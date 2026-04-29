---
title: "Ho scritto un simulatore di rete in C, ma ha piu problemi di memoria di Windows"
date: 2026-04-29 11:50:14 +0200
categories: [Computer Science,Reti]
tags: ["c"]
math: true
---


## Intro

Come esercizio per il laboratorio di Reti ci è stato assegnato di scrivere un simulatore dello stack TCP/IP in un linguaggio ad oggetti. Ovviamente l'ho fatto in **C**.

Mi diletto nell'arte di scrivere codice in C da circa 3 mesi ormai, soprattutto per la **crittografia** e la GPGPU, ma non ho ancora messo il piede in progetti da più di due files, e in **reali programmi** effettivamente.

Questo esercizio dimostra la mia acerbità nel linguaggio, che spero nel tempo migliori. Per pubblicare questo programma avrei probabilmente dovuto passare altro tempo a scrivere i `free()` e migliorare in generale la gestione della memoria, cosa che avrei fatto volentieri.
Ma una volta presentato il progetto ho pensato di andare avanti. Ho molte cose che bollono in pentola, insieme alla crittografia mi vorrei dilettare alla scrittura di **piccole app per Flipper** e mi piacerebbe sviscerare la **CPTS** finchè posso permettermela a 7 euro al mese, ma di solito sono il tipo di persona da una cosa alla volta.

Il codice: puoi giudicarlo, non ha sentimenti. Se vedi un `free()`, sicuramente stai avendo delle allucinazioni.

Ma permettimi di dire. Dopo aver scritto 800 linee di codice in 5 giorni, risolvendo un problema alla volta, **incastrando i pezzi del puzzle** in C come si fa da più di due decenni, sento che questo codice merita la luce del Sole. Non c'è malizia nei suoi problemi di memoria, solo ingenuità.

Prima di 3 mesi fa, non potevo mai sperare di scrivere 800 linee di codice in C in così poco tempo, anche se solo semi-funzionante. Che sia l'inizio di una nuova (utilissima) skill. 

```c
/*
* NetMock by matteocapo
* simulatore dello stack TCP/IP
*/

/*
 * Bug attuali:
 * - Ogni tanto il contatore di pacchetti fa cilecca
 * - segfault se il contatore di pacchetti fa cilecca
 * - il numero di frame è mockato per leggere
 * - no other known bugs except totally volontary memory corruption bugs. and missing free 
 *
 * Modifiche da fare:
 * - mettere un MAC di default e la porta solo all'invio di messaggi
 * - refactoring del codice brutto (circa metà)
 *
 */

#include <string.h>
#include <stdlib.h>
#include <stdint.h>
#include <stdio.h>
#include <sys/types.h>

#define UDP_HEADER_SIZE 8
#define MTU_SIZE 1500

typedef struct {
    uint8_t* data;
    uint8_t* transportHeader;
    uint8_t* networkHeader;
} Packet;

typedef struct {    
    uint8_t header[14];
    uint8_t* payload;
    uint8_t* trailer;
} Frame;

typedef struct {
    uint32_t ip;
    uint16_t port;
    uint8_t mac[6];
    Frame* frames;
    int num_frames;
} Node;

typedef struct {
    Node* node;
} Nodes;

// Global instance of network
Nodes nodes[10];
int idx = 0;


/* TUI */
void tui();
void crea_un_host();
void invia_un_messaggio();
void leggi_i_messaggi();
void invia_un_messaggio_lungo();
void leggi_un_frame();
void quanti_pacchetti();

void sendMsg(Node* host1, Node* host2, char* payload);
void sendFrames(Frame* frames, Node* dst, int n);

/* wrappers for encapsulation */
void encapsulate(Packet* pkt, uint8_t* payload, Node* src, Node* dst);
void fragment(Frame* frames, Packet* pkt, Node* src, Node* dst, int n);

/* specific implementation of encapsulate and fragment */
void encapsulateData(Packet* pkt, uint8_t* payload);
int encapsulateTransport(Node* src, Node* dst, Packet* pkt);
void encapsulateNetwork(Node* src, Node* dst, Packet* pkt, int protocol);
uint8_t* serializePacket(Packet* pkt, int idx);

/* read packet contents */
void print_messages(Node* node);
int packets_number(Node* node);

/* read frame contents */
void print_frame(Frame* frame);
Packet* reconstruct_frames(Node* node);

/* utilities */
Node* createHost(char* ip, char* port, char* mac);
void add_node(Node* node);
void print_hosts();
void hexprint(uint8_t* str, int n);
int string_to_ip(uint32_t* real_ip, char* ip);
int string_to_port(uint16_t* real_port, char* port);
int string_to_mac(uint8_t* real_mac, char* mac);
char* ip_to_string(uint32_t ip);
int index_frame2packet(int idx);
int exceeding_bytes_packet(Packet* pkt);
int len_network_header(Packet* pkt);
int len_transport_header(Packet* pkt);
int len_data_wo_headers(Packet* pkt);
int count_frames(Packet* pkt);
char* clean_string(char* str);
uint16_t calculate_checksum(Packet* pkt);



int main(int argc, char* argv[]){
    tui();
    return 0;
}


void tui(){
    printf("Benvenuto in NetMock!\n");
    while(1){
        printf("Cosa vuoi fare?\n0 - Creare un Host\n1 - Inviare un messaggio\n2 - Inviare un messaggio lungo\n3 - Leggere i messaggi\n4 - Leggere gli header di un pacchetto\n5 - Numero di pacchetti ricevuti\n> ");
        char scelta[2];
        scanf("%1s", scelta);
        switch (atoi(scelta)) {
            case 0:
                crea_un_host();
                break;

            case 1:
                invia_un_messaggio();
                break;

            case 2:
                invia_un_messaggio_lungo();
                break;

            case 3:
                leggi_i_messaggi();
                break;

            case 4:
                leggi_un_frame();
                break;

            case 5:
                quanti_pacchetti();
                break;            

            default:
                break;

        }
    }
}


void crea_un_host(){
    printf("inserisci IP address:\n");
    char ip[17];
    scanf("%16s", ip);
    
    printf("inserisci MAC address:\n");
    char mac[18];
    scanf("%17s", mac);

    printf("inserisci Porta:\n");
    char port[6];
    scanf("%5s", port);

    Node* n = createHost(ip,port,mac);
    if (n != NULL){ add_node(n); }
}


void invia_un_messaggio(){
    printf("Seleziona il mittente:\n");
    print_hosts();
    char mittente[2];
    scanf("%1s", mittente);
     
    printf("Seleziona il destinatario:\n");
    print_hosts();
    char destinatario[2];
    scanf("%1s", destinatario);

    printf("Scrivi il messaggio che vuoi inviare\n");
    char buffer[0xff00];
    scanf("%65279s", buffer);

    int m = atoi(mittente);
    int d = atoi(destinatario);
    if ((m > idx) || (d > idx) || (m < 0) || (d < 0)){
        printf("Index out of range\n");
        return;
    }
    sendMsg(nodes[m].node, nodes[d].node, buffer);
    return;
}


void leggi_i_messaggi(){
    printf("Seleziona l'host:\n");
    print_hosts();
    char mittente[2];
    scanf("%1s", mittente);
    int i = atoi(mittente);
    if ((i > idx-1) || (i < 0) ){
        printf("Index out of range\n");
        return;
    }
    print_messages(nodes[i].node);
    return;
}

void invia_un_messaggio_lungo(){
    printf("Seleziona il mittente:\n");
    print_hosts();
    char mittente[2];
    scanf("%1s", mittente);
     
    printf("Seleziona il destinatario:\n");
    print_hosts();
    char destinatario[2];
    scanf("%1s", destinatario);

    printf("Scrivi il messaggio che vuoi inviare\n");
    char buffer[0xff00];
    scanf("%65279s", buffer);

    int m = atoi(mittente);
    int d = atoi(destinatario);
    if ((m > idx) || (d > idx) || (m < 0) || (d < 0)){
        printf("Index out of range\n");
        return;
    }

    printf("Inserisci quante volte vuoi moltiplicare il buffer (max: 99999)\n> ");
    char mul[6];
    scanf("%5s", mul);
    int mu = atoi(mul);
    if (mu < 0 || mu > 99999) {
        printf("Grandezza del moltiplicatore errata\n");
        return;
    }

    int len = (strlen(buffer)*mu) + 1;
    char* messaggio = malloc(len);
    messaggio[0] = '\0';
    for (int i = 0 ; i < mu ; i++ ) {
        strcat(messaggio, buffer);
    }

    sendMsg(nodes[m].node, nodes[d].node, messaggio);
    free(messaggio);
    return;
}

void leggi_un_frame(){
    //TODO: demock and add check for index (packet number)
    printf("Seleziona l'host:\n");
    print_hosts();
    char id[2];
    scanf("%1s", id);
    int i = atoi(id);
    if ((i > idx) || (i < 0) ){
        printf("Index out of range\n");
        return;
    }
    printf("Seleziona il messaggio:\n");
    print_messages(nodes[i].node);
    printf(">");
    char msg[4];
    scanf("%3s", msg);
    int m = atoi(msg);
    // TODO: check index here
    // int index = index_frame2packet(m);
    print_frame(&nodes[i].node->frames[m]);
    return;
}


void quanti_pacchetti(){
    printf("Seleziona l'host:\n");
    print_hosts();
    char mittente[2];
    scanf("%1s", mittente);
    int i = atoi(mittente);
    if ((i > idx) || (i < 0) ){
        printf("Index out of range\n");
        return;
    }
    printf("Il numero di pacchetti arrivati al nodo è: %d\n", packets_number(&nodes->node[i]));
    return;
}


void sendMsg(Node* src, Node* dst, char* payload){
    // manda messaggio 
    
    Packet* pkt = malloc(sizeof(Packet));
    encapsulate(pkt, payload, src, dst);

    int n = count_frames(pkt);

    Frame* frames =(Frame*) malloc(sizeof(Frame)*n);
    fragment(frames, pkt, src, dst, n);

    sendFrames(frames, dst, n);
    dst->num_frames = dst->num_frames + n;
}


void sendFrames(Frame* frames, Node* dst, int n){
    // send Frame
    int nf = dst->num_frames;
    dst->frames = realloc(dst->frames, (dst->num_frames+n)*sizeof(Frame));
    for (int i = nf; i < n+nf; i++) {
        dst->frames[i] = frames[i-nf];
    }
}


void encapsulate(Packet* pkt, uint8_t* payload, Node* src, Node* dst){
    // encapsulation process, from data to packet
    
    // from payload to data
    encapsulateData(pkt, payload);
    // from data to datagram
    int protocol = encapsulateTransport(src, dst, pkt);
    //from datagram to packet
    if(protocol){
        encapsulateNetwork(src, dst, pkt, protocol);
    }
}


void fragment(Frame* frames, Packet* pkt, Node* src, Node* dst, int n){

    uint16_t ether_type = 0x0800;

    for (int i = 0 ; i < n ; i++) {
        // DLL header
        memcpy(frames[i].header, src->mac, 6);
        memcpy(frames[i].header+6, dst->mac, 6);
        memcpy(frames[i].header+12, &ether_type, 2);
        // payload 
        int bytes = i == n-1 ? exceeding_bytes_packet(pkt) : MTU_SIZE;
        frames[i].payload = malloc(bytes);
        memcpy(frames[i].payload,serializePacket(pkt,i), bytes);
        // Trailer
        uint8_t CRC[4] = {0,0,0,0};
        frames[i].trailer = malloc(4);
        memcpy(frames[i].trailer, CRC, 4);
    }
}


void encapsulateData(Packet* pkt, uint8_t* payload){
    // inizializza pkt->data con il contenuto del messaggio, senza limite di grandezza
    // segmenteremo dopo in pacchetti
    pkt->data = malloc(strlen(payload)+1);
    memcpy(pkt->data, payload, strlen(payload));
    pkt->data[strlen(payload)] = '\0';
}


int encapsulateTransport(Node* src, Node* dst, Packet* pkt){

    // copia src->port e dst->port in pkt->transportHeader: 32 bit
    pkt->transportHeader = malloc(sizeof(uint8_t)*UDP_HEADER_SIZE);
    memcpy(pkt->transportHeader, &src->port, sizeof(uint16_t));
    memcpy(pkt->transportHeader+2, &dst->port, sizeof(uint16_t));
    
    // aggiungiamo il campo lunghezza e checksum: 16 e 16 bit
    int ilen = strlen(pkt->data);
    if (ilen > (0xff00)){
        printf("mbare ma che ci devi fare con 65280 bytes ma accuppanaggiti\n");
        return 0;
    }

    uint16_t len = ilen;
    uint16_t checksum = calculate_checksum(pkt);

    memcpy(pkt->transportHeader+4, &len, sizeof(uint16_t));
    memcpy(pkt->transportHeader+6, &checksum, sizeof(uint16_t));


    return 17;
}


void encapsulateNetwork(Node* src, Node* dst, Packet* pkt, int protocol){
    uint8_t v4_ihl, ttl = 1, dspc_ecn = 0, p = protocol;
    uint16_t flags_fragmentoffset = 0b0100000000000000;
    uint16_t identification = 0, checksum = 0, lenNetwork;

    pkt->networkHeader = malloc(sizeof(uint8_t)*20);
    
    // version 4 bit + Internet header length 4 bit : 8 bit
    v4_ihl = v4_ihl ^ v4_ihl ^ 5 ^ (4 << 4);
    memcpy(pkt->networkHeader, &v4_ihl, sizeof(uint8_t));

    // dspc 6 bit + ecn 2 bit : 8 bit
    memcpy(pkt->networkHeader+1, &dspc_ecn, sizeof(uint8_t));

    // total length : 16 bit
    memcpy(&lenNetwork, pkt->transportHeader+4, 2);
    if (protocol == 17){
        lenNetwork = lenNetwork /* len of payload */
                    + UDP_HEADER_SIZE /* len of UDP headers */
                    + 20; /* len of IP headers */        
    }
    memcpy(pkt->networkHeader+2, &lenNetwork, sizeof(uint16_t));

    // identification : 16 bit
    memcpy(pkt->networkHeader+4, &identification, sizeof(uint16_t));
    
    // flags 4 bit + fragmentoffset 12 bit : 16 bit
    memcpy(pkt->networkHeader+6, &flags_fragmentoffset, sizeof(uint16_t));

    // ttl : 8 bit
    memcpy(pkt->networkHeader+8, &ttl, sizeof(uint8_t));

    // protocol (17) : 8 bit
    memcpy(pkt->networkHeader+9, &p, sizeof(uint8_t));

    // checksum : 16 bit
    memcpy(pkt->networkHeader+10, &checksum, sizeof(uint16_t));

    // src address : 32 bit
    memcpy(pkt->networkHeader+12, &src->ip, sizeof(uint32_t));

    // dst address : 32 bit
    memcpy(pkt->networkHeader+16, &dst->ip, sizeof(uint32_t));

}


uint8_t* serializePacket(Packet* pkt, int idx){
    // prende un pacchetto intero e il numero della frame, 
    // ritorna i bytes da inserire nella frame
    // rendere agnostico rispetto al tipo
    
    int lenHeaders = len_network_header(pkt) + len_transport_header(pkt);

    int bytesTot = idx == count_frames(pkt)-1 ? exceeding_bytes_packet(pkt): MTU_SIZE;

    uint8_t* memory = malloc(bytesTot);

    int bytesToCopy = bytesTot - lenHeaders;

    memcpy(memory, pkt->networkHeader, len_network_header(pkt));
    memcpy(memory+len_network_header(pkt), pkt->transportHeader, len_transport_header(pkt));

    int offset = idx*len_data_wo_headers(pkt);
    memcpy(memory+lenHeaders,pkt->data+offset, bytesToCopy);

    return memory;
}


void print_messages(Node* node){

    Packet* packets = (Packet*) malloc(sizeof(Packet)*node->num_frames);
    packets = reconstruct_frames(node);
    for (int i = 0 ; i < packets_number(node); i++) {
        printf("[%d]: %s\n", i, packets[i].data);
    }
    return;
}


int packets_number(Node* node){
    // check len of frame from index network (contains len of headers too)
    // TODO: solve error of 2 long message (sembra risolto)
    int np = 0;
    for (int fi = 0; fi < node->num_frames; ){
        uint16_t len = 0;
        memcpy(&len, node->frames[fi].payload+2, sizeof(uint16_t));
        if (len > MTU_SIZE){
            int bytes_counter = len;
            // come trovo la terminazione del payload? 
            // gli leviamo il numero di bytes degli header
            uint8_t ihl;
            memcpy(&ihl, node->frames[fi].payload, 1);
            ihl = (ihl & 0x0f);
            bytes_counter -= (ihl*4) + UDP_HEADER_SIZE;

            // aspettiamo che arriva a zero levando il payload e incrementando fi
            while(bytes_counter > 0){
                if (bytes_counter - (MTU_SIZE - ((ihl*4) + UDP_HEADER_SIZE)) > 0) fi++;
                bytes_counter -= (MTU_SIZE - ((ihl*4) + UDP_HEADER_SIZE));
            }
        }
        np++;
        fi++;
    }
    return np;
}


void print_frame(Frame* frame){
    uint8_t ihl, ttl, dspc, ecn, protocol, p, version, tmp;
    uint16_t flags, fragmentoffset, tmp2, src, dst, identification, checksum, lenNetwork, lenTransport;
    uint32_t src_ip, dst_ip;
    // 1. read MAC
    
    printf("\n --- STAMPA FRAME --- \n\n");
    
    printf("\n --- DLL --- \n");
    printf("SRC MAC Address:\n");
    for (int i = 0 ; i < 6 ; i ++) {
        printf("0x%.2x ", frame->header[i]);
    }
    printf("\n");

    printf("DST MAC Address:\n");
    for (int i = 6 ; i < 12 ; i ++) {
        printf("0x%.2x ", frame->header[i]);
    }
    printf("\n");

    //2. read payload 

    printf("\n --- NETWORK --- \n");

    memcpy(&protocol, frame->payload, 1);
    version = (protocol & 0xf0) >> 4;
    printf("IP version:\n%d\n", version);

    ihl = (protocol & 0x0f);
    printf("Internet Header Len:\n%d\n", ihl);

    memcpy(&tmp,frame->payload+1, sizeof(uint8_t));
    dspc = (tmp & 0xf0) >> 4;
    printf("DSPC:\n%d\n", dspc);
    
    ecn = (tmp & 0x0f);
    printf("ECN:\n%d\n", ecn);

    memcpy(&lenNetwork,frame->payload+2, sizeof(uint16_t));
    printf("Total Packet Len:\n%d\n", lenNetwork);

    memcpy(&identification,frame->payload+4, sizeof(uint16_t));
    printf("Identification:\n%d\n", identification);

    memcpy(&tmp2,frame->payload+6, sizeof(uint16_t));
    flags = (tmp2 & 0b1110000000000000) >> 13;
    printf("Flags:\n%.3b\n", flags);
    fragmentoffset = (tmp2 & 0b0001111111111111);
    printf("Offset:\n%d\n", fragmentoffset);

    memcpy(&ttl,frame->payload+8, sizeof(uint8_t));
    printf("TTL:\n%d\n", ttl);

    memcpy(&p,frame->payload+9, sizeof(uint8_t));
    printf("Transport Protocol:\n%d\n", p);

    memcpy(&checksum,frame->payload+10, sizeof(uint16_t));
    printf("Checksum:\n%d\n", checksum);

    memcpy(&src_ip,frame->payload+12, sizeof(uint32_t));
    printf("SRC IP:\n%s\n", ip_to_string(src_ip));

    memcpy(&dst_ip,frame->payload+16, sizeof(uint32_t));
    printf("DST IP:\n%s\n", ip_to_string(dst_ip));

    printf("\n --- TRANSPORT --- \n");
    memcpy(&src, frame->payload+20, sizeof(uint16_t));
    printf("SRC PORT:\n%d\n", src);

    memcpy(&dst, frame->payload+22, sizeof(uint16_t));
    printf("DST PORT:\n%d\n", dst);

    memcpy(&lenTransport, frame->payload+24, sizeof(uint16_t));
    printf("Data Length:\n%d\n", lenTransport);

    memcpy(&checksum, frame->payload+26, sizeof(uint16_t));
    printf("Checksum:\n%d\n", checksum);
}


Packet* reconstruct_frames(Node* node){
    // ricostruisci i pacchetti interi data una lista di frames 
    uint16_t len;
    int pi = 0;
    Packet* packets = (Packet*) malloc(sizeof(Packet)*node->num_frames);


    for (int fi = 0 ; fi < node->num_frames; fi++) {
        memcpy(&len, node->frames[fi].payload+2, sizeof(uint16_t));
        if (len > MTU_SIZE){
            // ricostruisci il pacchetto con gli header di uno e il payload della sequenza.
            uint8_t ihl;
            memcpy(&ihl, node->frames[fi].payload, 1);
            ihl = (ihl & 0x0f);
            packets[pi].networkHeader = malloc(ihl*4); 
            memcpy(packets[pi].networkHeader, node->frames[fi].payload, ihl*4);
            packets[pi].transportHeader = malloc(UDP_HEADER_SIZE); 
            memcpy(packets[pi].transportHeader, node->frames[fi].payload+(ihl*4), UDP_HEADER_SIZE);
            int bytes_len = 0, offset = 0;
            packets[pi].data = malloc(len - (((ihl)*4) + UDP_HEADER_SIZE));
            while(bytes_len != (len - ((ihl*4) + UDP_HEADER_SIZE))){
                memcpy(packets[pi].data+bytes_len, node->frames[fi].payload+(ihl*4)+UDP_HEADER_SIZE+(offset), 1);
                bytes_len++;
                offset++;
                if (offset == (MTU_SIZE-((ihl*4) + UDP_HEADER_SIZE))) {
                    offset = 0;
                    fi++;
                }
            }
        }
        else {
            // inserisci il frame in un pacchetto 
            uint8_t ihl;
            memcpy(&ihl, node->frames[fi].payload, 1);
            ihl = (ihl & 0x0f);
            packets[pi].networkHeader = malloc(ihl*4); 
            memcpy(packets[pi].networkHeader, node->frames[fi].payload, ihl*4);
            packets[pi].transportHeader = malloc(UDP_HEADER_SIZE); 
            memcpy(packets[pi].transportHeader, node->frames[fi].payload+(ihl*4), UDP_HEADER_SIZE);
            packets[pi].data = malloc(len-((ihl*4)+UDP_HEADER_SIZE));
            memcpy(packets[pi].data, node->frames[fi].payload+(ihl*4)+UDP_HEADER_SIZE,len-((ihl*4)+UDP_HEADER_SIZE));
        }
        pi++;
    }
    return packets;
}


Node* createHost(char* ip, char* port, char* mac){
    Node* n = malloc(sizeof(Node));
    uint8_t real_mac[6];
    uint32_t real_ip;
    uint16_t real_port;
    if (string_to_ip(&real_ip,ip) == 0) {return NULL;}
    if (string_to_port(&real_port,port) == 0) {return NULL;}
    if (string_to_mac(real_mac,mac) == 0) {return NULL;}
    n->ip = real_ip;
    n->port = real_port;
    memcpy(n->mac, real_mac, 6);
    n->frames = (Frame*) malloc(sizeof(Frame));
    n->num_frames = 0;
    return n;
}


void add_node(Node* node){
    nodes[idx++].node = node;
}


void print_hosts(){
    for (int i = 0 ; i < idx ; i++) {
        printf("[%d] - %s\n", i, ip_to_string(nodes[i].node->ip));
    }
}


void hexprint(uint8_t* str, int n){
    // stampa n bytes in esadecimale
    for (int i = 0; i < n; i++) {
        printf("0x%.2x ",str[i]);
    }
    printf("\n");
}


int string_to_ip(uint32_t* real_ip, char* ip){
    // da char a 4 int -> 4 uint8_t to uint32_t
    if(strlen(ip) < 7) {
        printf("Invalid IP\n");
        return 0;
    }

    char* ip_copy = strdup(ip);
    uint8_t tmpint[4];

    char* tmpchr = strtok(ip_copy,".");
    for (int i = 0; i < 4; i ++) {
        tmpint[i] = atoi(tmpchr); 
        if (atoi(tmpchr) < 0 || atoi(tmpchr) > 255) {
            goto InvalidIP;
        }
        tmpchr = strtok(NULL,".");
    }
    *real_ip = *real_ip ^ *real_ip;
    *real_ip = tmpint[0] ^ (tmpint[1] << 8) ^ (tmpint[2] << 16) ^ (tmpint[3] << 24);
    goto FreeAndReturn;

InvalidIP:
    printf("Invalid IP\n");
    free(ip_copy);
    return 0;
FreeAndReturn:
    free(ip_copy);
    return 1;
}


int string_to_port(uint16_t* real_port, char* port){
    // da stringa a uint16_t
    *real_port = atoi(port);
    if ((*real_port == 0) || (*real_port < 0) || (*real_port > 65535)){
        printf("Invalid Port\n");
        return 0;
    }
    return 1;
}


int string_to_mac(uint8_t* real_mac, char* mac){
    // da char a 6 int -> 6 uint8_t to uint8_t[6]
    if(strlen(mac) < 17){
        printf("Invalid MAC\n");
        return 0;
    }

    char* mac_copy = strdup(mac);
    char* tmpchr = strtok(mac_copy,":");

    for (int i = 0; i < 6; i ++) {
        uint8_t tmpint;
        tmpint = strtol(tmpchr, NULL, 16); 
        if (strtol(tmpchr, NULL, 16) < 0 || strtol(tmpchr, NULL, 16) > 255) {
            goto InvalidMAC;
        }
        real_mac[i] = real_mac[i] ^ real_mac[i] ^ tmpint;
        tmpchr = strtok(NULL,":");
    }
    goto FreeAndReturn;

InvalidMAC:
    printf("Invalid MAC\n");
    free(mac_copy);
    return 0;
FreeAndReturn:
    free(mac_copy);
    return 1;
}


char* ip_to_string(uint32_t ip){
    char* ip_string = malloc(16);
    uint32_t tmp = 0;
    memcpy(&tmp, &ip, 4);
    snprintf(ip_string, 17,"%d.%d.%d.%d", tmp&0xff, (tmp&0xff00)>>8, (tmp&0xff0000)>>16, (tmp&0xff000000)>>24);
    return ip_string;
}


int index_frame2packet(int idx){
   return 0; 
}


int exceeding_bytes_packet(Packet* pkt){
    // prende la lunghezza del paccketto compresi gli header
    uint16_t len = 0;
    memcpy(&len, pkt->networkHeader+2, 2);
    
    return len - (1472 * (len / 1472)) + len_network_header(pkt) + len_transport_header(pkt);
}


int len_network_header(Packet* pkt){
    uint8_t lenNet; 
    memcpy(&lenNet, pkt->networkHeader, 1);
    return (lenNet & 0x0f) * 4;
}


int len_transport_header(Packet* pkt){
    uint8_t lenTra;
    memcpy(&lenTra, pkt->networkHeader+9, 1);
    return lenTra == 17 ? UDP_HEADER_SIZE : 0;
}


int len_data_wo_headers(Packet* pkt){
    return MTU_SIZE - len_network_header(pkt) - len_transport_header(pkt);
}

int count_frames(Packet* pkt){
    uint16_t len;
    memcpy(&len, pkt->networkHeader+2, 2);
    return (((int)len + MTU_SIZE - 1) / MTU_SIZE);
}


char* clean_string(char* str){
    char* stringa = malloc(strlen(str)+1);
    stringa = strdup(str);
    if (stringa[strlen(stringa)-1] == '\n'){
        stringa[strlen(stringa)-1] = '\0';
    }
    return stringa;
}


uint16_t calculate_checksum(Packet* pkt){
    // Checksum is the 16-bit one's complement of the one's complement sum of a
    // pseudo header of information from the IP header, the UDP header, and the
    // data,  padded  with zero octets  at the end (if  necessary)  to  make  a
    // multiple of two octets.
    //
    // The pseudo  header  conceptually prefixed to the UDP header contains the
    // source  address,  the destination  address,  the protocol,  and the  UDP
    // length.   This information gives protection against misrouted datagrams.
    // This checksum procedure is the same as is used in TCP.
    //
    // If the computed  checksum  is zero,  it is transmitted  as all ones (the
    // equivalent  in one's complement  arithmetic).   An all zero  transmitted
    // checksum  value means that the transmitter  generated  no checksum  (for
    // debugging or for higher level protocols that don't care).
    // quando c'ho voglia, per ora appliciamo don't care
    return (uint16_t)0;
}




```
