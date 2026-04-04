---
title: "Algoritmi Crittografici"
date: 2026-03-22 19:23:24 +0100
categories: [Cryptography,Codici]
tags: ["c", "libgmp", "tonellishanks", "AES"]
math: true
---



Questo articolo contiene snippets di codice prodotto a scopo accademico per la comprensione di algoritmi crittografici. Alcuni esempi sono: Algoritmo di Tonelli-Shanks, Chinese Remainder Theorem, AES.

## Algoritmo di Tonelli-Shanks

### Headers
```c
#include <gmp.h>
#include <time.h>

void tonelliShanks(mpz_t res, mpz_t n, mpz_t p);
int isXmodN(mpz_t val, int x, int n);
void sqrtmodFermat(mpz_t res, mpz_t qr, mpz_t p);
void factorOddPart(mpz_t q, mpz_t s, mpz_t p);
void findQuadraticNonResidue(mpz_t z, mpz_t p);
void repeatedSquaring(mpz_t i, mpz_t t, mpz_t m, mpz_t p);
```

### Functions
```c
void tonelliShanks(mpz_t res, mpz_t n, mpz_t p) {
  // check if p is prime
  if (mpz_probab_prime_p(p, PRIME_TRIES) == 0) {
    gmp_printf("p = %Zd is definately not prime!", p);
    return;
  }

  // check if n is quadratic residue
  if (mpz_legendre(n, p) == -1) {
    gmp_printf("n = %Zd is not a quadratic residue\n", n);
    return;
  }

  // if p = 3 mod 4 use fermat calculation
  if (isXmodN(p, 3, 4) == 1) {
    sqrtmodFermat(res, n, p);
    return;
  }

  // initialize variables
  mpz_t q, s, z, t, r, c, m, i, b, exp_temp;
  mpz_inits(q, s, z, t, r, c, m, i, b, exp_temp, NULL);

  // find Q and S such that p-1 = Q*2^S con Q dispari
  factorOddPart(q, s, p);

  // find a Z that is not a quadratic residue
  findQuadraticNonResidue(z, p);

  // t = n ** Q mod p
  mpz_powm(t, n, q, p);

  // r = n ** (q+1)/2 mod p
  mpz_set_ui(r, 1);
  mpz_add(r, q, r);
  mpz_tdiv_q_ui(r, r, 2);
  mpz_powm(r, n, r, p);

  // c = z**q mod p
  mpz_powm(c, z, q, p);

  // m = s
  mpz_set(m, s);

  // iterate
  while (mpz_cmp_ui(t, 1) != 0) {
    // if t == 0 return r = 0
    if (mpz_cmp_ui(t, 0) == 0) {
      mpz_set_ui(res, 0);
      mpz_clears(q, s, z, t, r, c, m, i, b, exp_temp, NULL);
      return;
    }
    // repeated squaring to find the least i < 0 < M
    repeatedSquaring(i, t, m, p);

    // b = c ** (2**(m-i-1)) mod p
    mpz_ui_pow_ui(exp_temp, 2, mpz_get_ui(m) - mpz_get_ui(i) - 1);
    mpz_powm(b, c, exp_temp, p);

    // r = r*b mod p
    mpz_mul(r, r, b);
    mpz_mod(r, r, p);

    // c = b**2
    mpz_powm_ui(c, b, 2, p);

    // t = t * b**2
    mpz_mul(t, t, c);
    mpz_mod(t, t, p);

    // set m = i
    mpz_set(m, i);
  }
  // result is r (and -r mod p)
  mpz_set(res, r);
  mpz_clears(q, s, z, t, r, c, m, i, b, exp_temp, NULL);
}

void repeatedSquaring(mpz_t i, mpz_t t, mpz_t m, mpz_t p) {
  // find the lowest i for that t ** (2**i) mod p = 1
  mpz_t res, exp;
  mpz_inits(exp, res, NULL);

  // il più piccolo i è il primo i
  for (int iter = 1; iter < mpz_get_ui(m); iter++) {

    // exp = 2 ** i
    mpz_ui_pow_ui(exp, 2, iter);

    // res = t ** (2 ** i) mod p
    mpz_powm(res, t, exp, p);

    // if res == 1 return iter
    if (mpz_cmp_ui(res, 1) == 0) {
      mpz_set_ui(i, iter);
      break;
    }
  }
  mpz_clears(exp, res, NULL);
}

void findQuadraticNonResidue(mpz_t z, mpz_t p) {
  // use random approach (efficient as 50% is !qr)
  gmp_randstate_t state;
  gmp_randinit_default(state);
  gmp_randseed_ui(state, time(0));

  do {
    mpz_urandomm(z, state, p);
  } while (mpz_legendre(z, p) != -1);

  gmp_randclear(state);
}

void factorOddPart(mpz_t q, mpz_t s, mpz_t p) {
  mpz_t p_meno_uno, due;
  mpz_inits(p_meno_uno, NULL);
  mpz_init_set_ui(due, 2);

  // p_meno_uno = p-1
  mpz_sub_ui(p_meno_uno, p, 1);

  // find Q and S such that p-1 = Q*2^S con Q dispa
  unsigned long int temp = mpz_remove(q, p_meno_uno, due);
  mpz_set_ui(s, temp);

  mpz_clears(p_meno_uno, due, NULL);
}

int isXmodN(mpz_t val, int x, int n) {
  mpz_t temp;
  mpz_init(temp);

  // temp = p mod 4
  mpz_mod_ui(temp, val, n);
  int result = (mpz_cmp_ui(temp, x) == 0) ? 1 : 0;

  mpz_clear(temp);
  return result;
}

void sqrtmodFermat(mpz_t res, mpz_t qr, mpz_t p) {
  // se p = 3 mod 4 -> p+1//4 + p+1//4 = p+1//2 = (p-1)+2//2 = (p-1)/2 + 1
  // quindi radice quadrata modulare = x ^ p+1//4
  mpz_t temp, exp;

  mpz_inits(temp, exp, NULL);

  // check p=3 mod4
  mpz_mod_ui(temp, p, 4);
  if (mpz_cmp_ui(temp, 3) == 0) {
    // res = qr ** (p+1)//4 mod p
    mpz_set(temp, p);
    mpz_add_ui(temp, p, 1);
    mpz_tdiv_q_ui(exp, temp, 4);
    mpz_powm(res, qr, exp, p);
  } else
    gmp_printf("p non è = 3 mod 4 (p mod 4 = %Zd)\n", temp);

  mpz_clears(temp, exp, NULL);
}
```

### Usage
```c
if (mpz_legendre(a, p) != -1) {
  tonelliShanks(res, a, p);
  gmp_printf("%Zd is a quadratic residue!\n", a);
  gmp_printf("I tried to calculate the sqrt...\n%Zd\n", res);
}
```


## Chinese Remainder Theorem (CRT)

### Headers

```c
#include <gmp.h>

void initmpzArray(mpz_t a[], int size);
void copympzArray(mpz_t a[], mpz_t b[], int size);
int chineseRemainder(mpz_t res, mpz_t bigN, mpz_t a[], mpz_t n[], int size);
```

### Functions

```c
int chineseRemainder(mpz_t res, mpz_t bigN, mpz_t a[], mpz_t n[], int size) {
  int status_code = 0;
  mpz_t m1, m2, a12, temp1, temp2;
  mpz_inits(a12, temp1, temp2, m1, m2, NULL);

  // copio A ed N per non modificare l'array originale
  mpz_t A[size], N[size];
  initmpzArray(A, size);
  initmpzArray(N, size);
  copympzArray(A, a, size);
  copympzArray(N, n, size);

  for (int i = 0; i < size - 1; i++) {
    // extended euclidean to find m1,m2 : m1n1 + m2n2 = 1
    mpz_gcdext(a12, m1, m2, N[i], N[i + 1]);
    if (mpz_cmp_ui(a12, 1) != 0) {
      status_code = -1;
      goto cleanup;
    }

    // a12 = a1*m2*n2+a2*m1*n1
    mpz_mul(temp1, N[i + 1], m2);
    mpz_mul(temp1, temp1, A[i]);
    mpz_mul(temp2, N[i], m1);
    mpz_mul(temp2, temp2, A[i + 1]);
    mpz_add(a12, temp1, temp2);

    // riduco il problema a k-1
    // metto la a successiva a a12
    mpz_set(A[i + 1], a12);

    // la n successiva è n1*n2
    mpz_mul(N[i + 1], N[i], N[i + 1]);

    // modulo per restare nel ring
    mpz_mod(A[i + 1], A[i + 1], N[i + 1]);
  }
  // set results
  mpz_set(res, A[size - 1]);
  mpz_set(bigN, N[size - 1]);

cleanup:
  // clears
  mpz_clears(a12, temp1, temp2, m1, m2, NULL);
  for (int i = 0; i < size; i++)
    mpz_clears(A[i], N[i], NULL);

  return status_code;
}

void initmpzArray(mpz_t a[], int size) {
  for (int i = 0; i < size; i++) {
    mpz_init(a[i]);
  }
}

void copympzArray(mpz_t a[], mpz_t b[], int size) {
  for (int i = 0; i < size; i++) {
    mpz_set(a[i], b[i]);
  }
}

```

### Usage

```c
// esempio per k = 3
mpz_t a[3], n[3];
for (int i = 0; i < 3; i++)
  mpz_init(a[i]);
mpz_set_ui(a[0], 2);
mpz_set_ui(a[1], 3);
mpz_set_ui(a[2], 5);

for (int i = 0; i < 3; i++)
  mpz_init(n[i]);
mpz_set_ui(n[0], 5);
mpz_set_ui(n[1], 11);
mpz_set_ui(n[2], 17);

mpz_t res, bigN;
mpz_inits(res, bigN, NULL);

chineseRemainder(res, bigN, a, n, 3);
gmp_printf("il risultato è %Zd (mod %Zd)\n", res, bigN); 
```


## AES128 block cipher 

### Headers

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
```

### Functions

```c
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

void invShiftRows(uint8_t state[4][4]){
    /* inverse of Shift Rows, basically row right shift of row index amount */
    for(int i = 1; i < 4; i++){
        for(int j = 0; j<i; j++){
            rCircularShift(state[i]);
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

### Usage

```c
    // Decryption
    uint8_t ct[16] = "\x25\xd0\x73\xe4\x67\x9a\x75\xc4\xa3\x2a\xde\x82\xca\x7d\x8b\x44";
    uint8_t key[16] = "\x59\x45\x4c\x4c\x4f\x57\x20\x53\x55\x42\x4d\x41\x52\x49\x4e\x45"; 
    uint8_t flag[17];
    decryptAES128block(flag, ct, key);
    flag[16] = '\0';
    printf("flag:\n%s \n",flag);
    
    // Encryption
    uint8_t ct2[16];
    uint8_t flag2[17] = "Everyday I'm Shu";
    encryptAES128block(ct2, flag2, key);
    decryptAES128block(flag2, ct2, key);
    printf("decrypted flag:\n%s \n", flag2);

```
