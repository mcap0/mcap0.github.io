---
title: "Ho riscritto AES in C"
date: 2026-06-06 09:28:27 +0200
categories: [Cryptography,Codici]
tags: ["C", "AES"]
math: true
---

[Codice](#codice)

## Struttura di RSA

AES 128 inizia da un key schedule algorithm (uno di quelli per ingrandire la key in entrata), partendo dal blocco diviso in 4x4 (ogni casella è un byte) in plaintext (stato 1) e applicando 10 round a questo stato iniziale. 
1. **KeyExpansion** ( o key schedule)
    dalla chiave in input da 128 bit, 11 chiavi da 128 bit diverse vengono generate, per essere usate nello step di AddRoundKey.
2. **Initial Key Addition**
    AddRoundKey - XOR tra lo stato e la prima RoundKey
3. **Round**
    questa procedura viene ripetuta 10 volte (9 main, 1 last)
    a) `SubBytes` -> viene usato una lookup table per sostituire il byte con S-box 
    b) `ShiftRows` -> le ultime tre righe vengono shiftate di 1,2 o 3 colonne
    c) `MixColumns` -> nei 9 main round (e non nel last) viene effettuata matrix multiplication delle 4 colonne dello stato, combinando i 4 byte di ogni colonna
    d) `AddRoundKey` -> viene fatto XOR di tutta la matrice con la chiave di round

### KeyExpansion

prende in input una chiave da 16 bytes e produce undici chiavi 4x4 chiamate **RoundKeys**, derivate dalla chiave iniziale.

### Initial Key addition

ha un singolo **AddRoundKey** step, fa lo XOR con lo stato attuale e la **RoundKey** corrente

AddRoundKey è anche l'ultimo step di ogni round
è l'unico momento in cui la chiave viene usata in AES, cioè che viene mescolata nello stato 

### SubBytes

provvede **confusion** (ennalidlbito diff)
primo step di ogni round
prendi un byte dalla state matrix e lo sostituisci con un byte differente in una 16x16 lookup table chiamata S-Box o Substitution box

La funzione di sostituzione, fast lookup, è effettivamente una scorciatoia per effettuare una funzione non lineare (perchè quelle lineari possono essere rotte) sui bytes di input.
Quello che fa è effettivamente l'inverso modulare nel campo di Galois 2**8 + un tweak affine per massimizzare la confusione. 

per quello che ho capito, la tabella non prende la chiave quandi la sbox è sempre la stessa per ongi trasformazione aes

https://www.samiam.org/galois.html

### ShiftRows e MixColumns

provvede **diffusion**
la diffusion ideale al cambiare di un bit è il 50% dei bit output, tramite avalanche effect
la substitution di prima effettivamente crea un po' di scrambling ma non lo distribuisce

**ShiftRows** -> tiene la prima riga uguale, la seconda shifta di uno a sinistra, la terza di due e la quarta di 3
    Questa effettivamente serve ad evitare che la colonna venga criptata autonomamente, che renderebbe AES un block cyphers per ogni colonna.

**MixColumns** -> fa una matrix multiplication tra una colonna e una matrice preset. ogni singolo byte di ogni colonna affligge ogni singolo byte della colonna in output

[https://www.samiam.org/mix-column.html](https://www.samiam.org/mix-column.html)
[https://en.wikipedia.org/wiki/Rijndael_MixColumns](https://en.wikipedia.org/wiki/Rijndael_MixColumns)

## Codice

```c
#include <string.h>
#include <stdint.h>
#include <stdio.h>
#include <sys/types.h>


void encryptAES128block(uint8_t ct[16], uint8_t pt[16], uint8_t k[16]);
void decryptAES128block(uint8_t pt[16], uint8_t ct[16], uint8_t k[16]);
void subBytes(uint8_t s[4][4]);
void shiftRows(uint8_t s[4][4]);
void invSubBytes(uint8_t s[4][4]);
void invShiftRows(uint8_t s[4][4]);
void mixColumns(uint8_t s[4][4]);
uint8_t xtime(uint8_t val);
void invMixColumns(uint8_t s[4][4]);
void extract_4x4(uint8_t roundKey[4][4], uint8_t keyColumns[44][4], int idx);
void addRoundKey(uint8_t s[4][4], uint8_t roundKey[4][4]);
void col_bytes2matrix(uint8_t s[4][4], uint8_t ct[16]);
uint8_t s_box(uint8_t item);
void appendRoundKey(uint8_t roundKey[44][4], uint8_t word[4], int idx);
void xor_4x1(uint8_t w1[4], uint8_t w2[4]);
void rCircularShift(uint8_t word[4]);
void lCircularShift(uint8_t word[4]);
void expandKey(uint8_t roundKey[44][4],uint8_t master_key[4][4]);
void bytes2matrix(uint8_t mat[4][4], uint8_t arr[16]) ;
void matrix2bytes(uint8_t arr[16], uint8_t mat[4][4]);

int main(int argc, char* argv[]){
    uint8_t ct[16] = "\x25\xd0\x73\xe4\x67\x9a\x75\xc4\xa3\x2a\xde\x82\xca\x7d\x8b\x44";
    uint8_t key[16] = "\x59\x45\x4c\x4c\x4f\x57\x20\x53\x55\x42\x4d\x41\x52\x49\x4e\x45"; 
    uint8_t flag[17];
    decryptAES128block(flag, ct, key);
    flag[16] = '\0';
    printf("flag:\n%s \n",flag);
    
    uint8_t ct2[16];
    uint8_t flag2[17] = "Everyday I'm Shu";
    /* -- TEST ENCRYPTION -- */
    encryptAES128block(ct2, flag2, key);
    decryptAES128block(flag2, ct2, key);
    printf("decrypted flag:\n%s \n", flag2);
}


void encryptAES128block(uint8_t ct[16], uint8_t pt[16], uint8_t k[16]){
    /* -- Performs a block AES encryption given the plaintext and the key -- */
    
    uint8_t key_m[4][4], keyColumns[44][4], state[4][4], rk[4][4];
    int iter_round = 1;

    /* -- plaintext to state -- */
    col_bytes2matrix(state, pt); 

    /* -- Expand Key procedure -- */
    bytes2matrix(key_m,k);
    expandKey(keyColumns, key_m);

    /* -- Initial AddRoundKey -- */
    extract_4x4(rk, keyColumns, iter_round);
    addRoundKey(state,rk);
    
    /* -- 10 ROUNDS -- */
    while(iter_round < 11){
        subBytes(state);
        shiftRows(state);
        extract_4x4(rk, keyColumns, ++iter_round);
        if (iter_round != 11) mixColumns(state);
        addRoundKey(state,rk);
    }
    matrix2bytes(ct, state);
}


void subBytes(uint8_t s[4][4]){
    for (int i = 0 ; i < 4 ; i++) {
        for (int j = 0 ; j < 4 ; j++) {
            s[i][j] = s_box(s[i][j]);
        }
    }
}

void shiftRows(uint8_t s[4][4]){
    for(int i = 1; i < 4; i++){
        for(int j = 0; j<i; j++){
            lCircularShift(s[i]);
        }
    }
}

void decryptAES128block(uint8_t pt[16], uint8_t ct[16], uint8_t k[16]){
    /* -- Performs a block AES decryption given the ciphertext and the key -- */
    uint8_t key_m[4][4], keyColumns[44][4], state[4][4], rk[4][4];
    int iter_round = 11;

    /* -- ciphertext to state -- */
    col_bytes2matrix(state, ct); 

    /* -- Expand Key procedure -- */
    bytes2matrix(key_m,k);
    expandKey(keyColumns, key_m);

    /* -- Initial AddRoundKey -- */
    extract_4x4(rk, keyColumns, iter_round);
    addRoundKey(state,rk);
    
    /* -- 10 ROUNDS -- */
    while(iter_round > 1){
        invShiftRows(state);
        invSubBytes(state);
        extract_4x4(rk, keyColumns, --iter_round);
        addRoundKey(state,rk);
        if (iter_round != 1) invMixColumns(state);
    }
    matrix2bytes(pt, state);
}

void invMixColumns(uint8_t s[4][4]){
    /* inverse of Mix Columns
     * matrix multiplication operation, columnwise, in the Rijndael field*/

    for (int i = 0 ; i < 4 ; i++) {
        uint8_t u = xtime(xtime(s[0][i] ^ s[2][i]));
        uint8_t v = xtime(xtime(s[1][i] ^ s[3][i]));
        s[0][i] = s[0][i] ^ u;
        s[1][i] = s[1][i] ^ v;
        s[2][i] = s[2][i] ^ u;
        s[3][i] = s[3][i] ^ v;
    }
    mixColumns(s);
}

void mixColumns(uint8_t s[4][4]){
    /* -- Matrix multiplication of a column with a preset matrix, in the 
     * Rijndael field, mainly provides diffusion*/
    for (int i = 0 ; i < 4 ; i++) {
        uint8_t t = s[0][i] ^ s[1][i] ^ s[2][i] ^ s[3][i];
        uint8_t u = s[0][i];
        s[0][i] = s[0][i] ^ t ^ xtime(s[0][i] ^ s[1][i]);
        s[1][i] = s[1][i] ^ t ^ xtime(s[1][i] ^ s[2][i]);
        s[2][i] = s[2][i] ^ t ^ xtime(s[2][i] ^ s[3][i]);
        s[3][i] = s[3][i] ^ t ^ xtime(s[3][i] ^ u);
    }
}

uint8_t xtime(uint8_t val){
    // learned from http://cs.ucsb.edu/~koc/cs178/projects/JT/aes.c
    if (val & 0x80){
        return ((( val << 1) ^ 0x1B) & 0xff);
    }
    else 
        return val << 1;
}

void invShiftRows(uint8_t s[4][4]){
    /* inverse of Shift Rows, basically row right shift of row index amount */
    for(int i = 1; i < 4; i++){
        for(int j = 0; j<i; j++){
            rCircularShift(s[i]);
        }
    }
}

void invSubBytes(uint8_t s[4][4]){
    /* -- uses inverse substitution_box to reverse the SubBytes operation -- */
    static const uint8_t inv_substitution_box[256] = {
        0x52, 0x09, 0x6A, 0xD5, 0x30, 0x36, 0xA5, 0x38, 0xBF, 0x40, 0xA3, 0x9E, 0x81, 0xF3, 0xD7, 0xFB,
        0x7C, 0xE3, 0x39, 0x82, 0x9B, 0x2F, 0xFF, 0x87, 0x34, 0x8E, 0x43, 0x44, 0xC4, 0xDE, 0xE9, 0xCB,
        0x54, 0x7B, 0x94, 0x32, 0xA6, 0xC2, 0x23, 0x3D, 0xEE, 0x4C, 0x95, 0x0B, 0x42, 0xFA, 0xC3, 0x4E,
        0x08, 0x2E, 0xA1, 0x66, 0x28, 0xD9, 0x24, 0xB2, 0x76, 0x5B, 0xA2, 0x49, 0x6D, 0x8B, 0xD1, 0x25,
        0x72, 0xF8, 0xF6, 0x64, 0x86, 0x68, 0x98, 0x16, 0xD4, 0xA4, 0x5C, 0xCC, 0x5D, 0x65, 0xB6, 0x92,
        0x6C, 0x70, 0x48, 0x50, 0xFD, 0xED, 0xB9, 0xDA, 0x5E, 0x15, 0x46, 0x57, 0xA7, 0x8D, 0x9D, 0x84,
        0x90, 0xD8, 0xAB, 0x00, 0x8C, 0xBC, 0xD3, 0x0A, 0xF7, 0xE4, 0x58, 0x05, 0xB8, 0xB3, 0x45, 0x06,
        0xD0, 0x2C, 0x1E, 0x8F, 0xCA, 0x3F, 0x0F, 0x02, 0xC1, 0xAF, 0xBD, 0x03, 0x01, 0x13, 0x8A, 0x6B,
        0x3A, 0x91, 0x11, 0x41, 0x4F, 0x67, 0xDC, 0xEA, 0x97, 0xF2, 0xCF, 0xCE, 0xF0, 0xB4, 0xE6, 0x73,
        0x96, 0xAC, 0x74, 0x22, 0xE7, 0xAD, 0x35, 0x85, 0xE2, 0xF9, 0x37, 0xE8, 0x1C, 0x75, 0xDF, 0x6E,
        0x47, 0xF1, 0x1A, 0x71, 0x1D, 0x29, 0xC5, 0x89, 0x6F, 0xB7, 0x62, 0x0E, 0xAA, 0x18, 0xBE, 0x1B,
        0xFC, 0x56, 0x3E, 0x4B, 0xC6, 0xD2, 0x79, 0x20, 0x9A, 0xDB, 0xC0, 0xFE, 0x78, 0xCD, 0x5A, 0xF4,
        0x1F, 0xDD, 0xA8, 0x33, 0x88, 0x07, 0xC7, 0x31, 0xB1, 0x12, 0x10, 0x59, 0x27, 0x80, 0xEC, 0x5F,
        0x60, 0x51, 0x7F, 0xA9, 0x19, 0xB5, 0x4A, 0x0D, 0x2D, 0xE5, 0x7A, 0x9F, 0x93, 0xC9, 0x9C, 0xEF,
        0xA0, 0xE0, 0x3B, 0x4D, 0xAE, 0x2A, 0xF5, 0xB0, 0xC8, 0xEB, 0xBB, 0x3C, 0x83, 0x53, 0x99, 0x61,
        0x17, 0x2B, 0x04, 0x7E, 0xBA, 0x77, 0xD6, 0x26, 0xE1, 0x69, 0x14, 0x63, 0x55, 0x21, 0x0C, 0x7D,
    };

    for (int i = 0 ; i < 4 ; i++) {
        for (int j = 0 ; j < 4 ; j++) {
            s[i][j] = inv_substitution_box[s[i][j]];
        }
    }
}

void extract_4x4(uint8_t roundKey[4][4], uint8_t keyColumns[44][4], int idx){
    /* -- Extracts the 4x4 matrix of a certain index from keyColumns -- */
    for (int i = 0 ; i < 4 ; i++) {
        for (int j = 0 ; j < 4 ; j++) {
            roundKey[i][j] = keyColumns[((idx-1)*4) + i][j]; 
        }
    }
}

void addRoundKey(uint8_t s[4][4], uint8_t roundKey[4][4]){
    /* -- XOR of the state matrix with the roundKey, same for inverse --*/
    for(int i = 0; i < 4; i++){
        for (int j = 0 ; j < 4 ; j++) {
            s[i][j] = s[i][j]^roundKey[j][i];
        }
    }
}

void col_bytes2matrix(uint8_t s[4][4], uint8_t ct[16]){
    /* -- bytes to matrix columnwise -- */
    int idx = 0;
    for (int i = 0; i < 4; i++) {
        for (int j = 0 ; j < 4 ; j++) {
            s[j][i] = ct[idx++];     
        }
    }
}

void expandKey(uint8_t roundKey[44][4],uint8_t master_key[4][4]){
    /* -- Schedule to derive 11 4x4 roundkeys from the AES key itself -- */
    //# Round constants https://en.wikipedia.org/wiki/AES_key_schedule#Round_constants
    static const uint8_t r_con[32] = {
        0x00, 0x01, 0x02, 0x04, 0x08, 0x10, 0x20, 0x40,
        0x80, 0x1B, 0x36, 0x6C, 0xD8, 0xAB, 0x4D, 0x9A,
        0x2F, 0x5E, 0xBC, 0x63, 0xC6, 0x97, 0x35, 0x6A,
        0xD4, 0xB3, 0x7D, 0xFA, 0xEF, 0xC5, 0x91, 0x39
        };
    int iteration_size = 4, i = 1;
    
    memcpy(roundKey, master_key, 16*sizeof(uint8_t));
    for(int idx = 4; idx < 44; idx++){
        uint8_t word[4];
        memcpy(word, roundKey[idx -1], 4);
        // Perform schedule_core once every "row".
        if (idx % iteration_size == 0) {
            lCircularShift(word);
            for (int j = 0; j < 4; j++) word[j] = s_box(word[j]);
            word[0] = word[0] ^ r_con[i++]; 
        }
        xor_4x1(word,roundKey[idx-4]);
        appendRoundKey(roundKey,word,idx);
    }
}


void appendRoundKey(uint8_t roundKey[44][4], uint8_t word[4], int idx){
    for (int i = 0; i < 4; i++)
        roundKey[idx][i] = word[i];
}

void xor_4x1(uint8_t w1[4], uint8_t w2[4]){
    for (int i = 0; i < 4; i++)
        w1[i] = w1[i] ^ w2[i];
}

uint8_t s_box(uint8_t item){
    static const uint8_t substitution_box[256] = {
        0x63, 0x7C, 0x77, 0x7B, 0xF2, 0x6B, 0x6F, 0xC5, 0x30, 0x01, 0x67, 0x2B, 0xFE, 0xD7, 0xAB, 0x76,
        0xCA, 0x82, 0xC9, 0x7D, 0xFA, 0x59, 0x47, 0xF0, 0xAD, 0xD4, 0xA2, 0xAF, 0x9C, 0xA4, 0x72, 0xC0,
        0xB7, 0xFD, 0x93, 0x26, 0x36, 0x3F, 0xF7, 0xCC, 0x34, 0xA5, 0xE5, 0xF1, 0x71, 0xD8, 0x31, 0x15,
        0x04, 0xC7, 0x23, 0xC3, 0x18, 0x96, 0x05, 0x9A, 0x07, 0x12, 0x80, 0xE2, 0xEB, 0x27, 0xB2, 0x75,
        0x09, 0x83, 0x2C, 0x1A, 0x1B, 0x6E, 0x5A, 0xA0, 0x52, 0x3B, 0xD6, 0xB3, 0x29, 0xE3, 0x2F, 0x84,
        0x53, 0xD1, 0x00, 0xED, 0x20, 0xFC, 0xB1, 0x5B, 0x6A, 0xCB, 0xBE, 0x39, 0x4A, 0x4C, 0x58, 0xCF,
        0xD0, 0xEF, 0xAA, 0xFB, 0x43, 0x4D, 0x33, 0x85, 0x45, 0xF9, 0x02, 0x7F, 0x50, 0x3C, 0x9F, 0xA8,
        0x51, 0xA3, 0x40, 0x8F, 0x92, 0x9D, 0x38, 0xF5, 0xBC, 0xB6, 0xDA, 0x21, 0x10, 0xFF, 0xF3, 0xD2,
        0xCD, 0x0C, 0x13, 0xEC, 0x5F, 0x97, 0x44, 0x17, 0xC4, 0xA7, 0x7E, 0x3D, 0x64, 0x5D, 0x19, 0x73,
        0x60, 0x81, 0x4F, 0xDC, 0x22, 0x2A, 0x90, 0x88, 0x46, 0xEE, 0xB8, 0x14, 0xDE, 0x5E, 0x0B, 0xDB,
        0xE0, 0x32, 0x3A, 0x0A, 0x49, 0x06, 0x24, 0x5C, 0xC2, 0xD3, 0xAC, 0x62, 0x91, 0x95, 0xE4, 0x79,
        0xE7, 0xC8, 0x37, 0x6D, 0x8D, 0xD5, 0x4E, 0xA9, 0x6C, 0x56, 0xF4, 0xEA, 0x65, 0x7A, 0xAE, 0x08,
        0xBA, 0x78, 0x25, 0x2E, 0x1C, 0xA6, 0xB4, 0xC6, 0xE8, 0xDD, 0x74, 0x1F, 0x4B, 0xBD, 0x8B, 0x8A,
        0x70, 0x3E, 0xB5, 0x66, 0x48, 0x03, 0xF6, 0x0E, 0x61, 0x35, 0x57, 0xB9, 0x86, 0xC1, 0x1D, 0x9E,
        0xE1, 0xF8, 0x98, 0x11, 0x69, 0xD9, 0x8E, 0x94, 0x9B, 0x1E, 0x87, 0xE9, 0xCE, 0x55, 0x28, 0xDF,
        0x8C, 0xA1, 0x89, 0x0D, 0xBF, 0xE6, 0x42, 0x68, 0x41, 0x99, 0x2D, 0x0F, 0xB0, 0x54, 0xBB, 0x16,
    } ;
    return substitution_box[(int)item]; 
}


void rCircularShift(uint8_t word[4]){
    uint8_t tmp;
    tmp = word[3];
    word[3] = word[2];
    word[2] = word[1];
    word[1] = word[0];
    word[0] = tmp;
}

void lCircularShift(uint8_t word[4]){
    uint8_t tmp;
    tmp = word[0];
    word[0] = word[1];
    word[1] = word[2];
    word[2] = word[3];
    word[3] = tmp;
}

void bytes2matrix(uint8_t mat[4][4], uint8_t arr[16]) {
    /* -- Converts a 16 byte array in a 4x4 matrix -- */
    int idx = 0;
    for(int i = 0; i < 4; i++){
        for(int j = 0; j < 4; j++){
            mat[i][j] = arr[idx++];
        }
    }   
}

void matrix2bytes(uint8_t arr[16], uint8_t mat[4][4]) {
    /* -- Converts a 4x4 matrix in a 16 byte array -- */
    int idx = 0;
    for(int i = 0; i < 4; i++){
        for(int j = 0; j < 4; j++){
            arr[idx++] = mat[j][i];
        }
    }
}

```
