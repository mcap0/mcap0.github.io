---
title: Reverse Engineering - Le basi per l'analisi malware
date: 2022-06-18 12:00:00 -500
categories: [Security Research, Malware Analysis]
tags: [reverse engineering, malware analysis]
---

In questo articolo vi guiderò tra i migliori metodi per aumentare di qualche livello le vostre skill da Reverse Engineer, vi spiegherò perché amo questo ruolo e in cosa consiste. Cercherò di essere sintetico e conciso, per poi presentarvi strumenti, tool e tecniche che sono fondamentali per l’analisi di malware.

Ammettiamolo, noi informatici possediamo questa innata voglia di scavare sotto la superficie, di ridurre e analizzare e comprendere a pieno come qualcosa funziona. Ci interessa capire come si muovono i suoi ingranaggi e in quanti modi strani potremmo sfruttare, modificare o rompere questi componenti.

Quando scrivo “qualcosa” intendo letteralmente qualsiasi cosa.

Per questo da piccolini quando ci hanno messo di fronte ad un computer è stato amore a prima vista. Non sapere come qualcosa funziona alimentava la nostra curiosità e questo dogma ci spinge ancora oggi in attività che ci piacciono e ci soddisfano.

Purtroppo un informatico su 5 è anche masochista e si cimenta in puzzle indecifrabili del calibro 100000 righe di linguaggio macchina ARM perché ‘siamo fatti così‘.

In questo articolo vi guiderò tra i migliori metodi per aumentare di qualche livello le vostre skill da Reverse Engineer, vi spiegherò perché amo questo ruolo e in cosa consste. Cercherò di essere sintetico e conciso, per poi presentarvi strumenti, tool e tecniche che sono fondamentali per l’analisi di malware.

Sarà inevitabile presentare delle basi di assembly, suggerisco vivamente di avere un’idea generale di tutte le componenti e funzioni della CPU, anche se manterrò una scrittura poco tecnica e comprensibile (spero).

## The best skill for malware analysis

Il Reverse Engineering più che un ruolo è un processo. Ogni giorno usiamo le nostre abilità da “analytical thinkers” per scomporre ogni problema che ci si presenta nelle sue componenti più elementari, e più le nostre conoscenze su un dato argomento sono acute, più accurata è la mappa mentale che ci creiamo.

Nell’informatica questo processo è generalmente attribuito alla capacità di analizzare e comprendere a pieno un’informazione, di tracciarne a ritroso il percorso che ha reso questa informazione tale.

In pratica riuscire ad estrapolare più dati e informazioni possibili da un artefatto. Questo nome è convenzionalmente dato all’oggetto di un analisi nel campo della sicurezza informatica.

Ma cosa bisogna cercare durante l’analisi di un artefatto?

## Come definire il processo di analisi

Da artista quale sei, il processo analitico in realtà dipende completamente da te.

Certo, a volte si ha un obiettivo mirato, ma spesso il vero reverse engineering lo effettuiamo anche senza accorgercene quando nelle nostre ricerche incontriamo un qualcosa di nuovo.

Allora cerchiamo su google, filtriamo le ricerche, seguiamo tutorial o guardiamo video per riuscire a collocare quel qualcosa di sconosciuto in tutto il contesto su cui stiamo lavorando.

Questo processo è una vera e propria skill che si acquisisce col tempo e la pratica, ed è alla base di moltissime tecniche che riguardano il nostro campo. In particolare nell’analisi di malware questa skill è strapagata e spesso anche abbastanza appagante.

Ci si ritrova sempre davanti a nuove e complesse sfide, inoltre con la pratica possiamo affinare contemporaneamente le nostre capacità risolutive, conoscenze tecnologiche e abilità di sopportazione. La pazienza è troppo sottovalutata.

## Tecniche e risorse per il Malware Analyst

In questa sezione vedremo come il Reverse Engineering si applica all’analisi malware. In particolare andremo a scomporre un file eseguibile con due software: gdb e radare2.

### Artefact Reverse Engineering

Il reverse engineering di un artefatto si basa sulla comprensione di ogni suo bit e byte.

Dato un file lo scopo dell’analisi in realtà va ben oltre la lettura di ogni bit che contiene. Dovremmo essere in grado di studiarne il funzionamento ed il comportamento, cioè le intenzioni di chi ha creato per primo l’oggetto di analisi, e spesso quest’ultimo ci viene presentato in un formato leggibile solo dai computer.

Anche per questo un numero non meglio definito di geni ha programmato strumenti come:

- Disassemblers 
    che scompongono file eseguibili in operazioni elementari (assembly code).
- Debuggers
    che aiutano nel tracciamento del “code flow” e nello studio del comportamento di un programma. Un debugger permette ad esempio di eseguire il programma un’istruzione alla volta, utile per la comprensione di certi codici.
- Decompilers
    strumenti che hanno il compito di provare ad estrapolare il codice sorgente originale di un programma compilato.
- Strumenti per analisi binaria
    permettono di identificare svariate informazioni grazie a bit specifici all’interno del file.
- Strumenti per il monitoraggio
    che aiutano a tenere traccia del comportamento del programma, come l’uso della memoria o delle interfacce network.

Con esempi pratici adesso ti guiderò in due analisi elementari per porre le basi di come un processo di reverse engineering si presenta.

## Basic Analysis | Vulnerable C code

Lo scope di questa analisi è un qualsiasi programma di cui abbiamo il codice sorgente in C. Una situazione inusuale penserai, ma è uno dei modi più veloci che ho trovato per entrare in familiarità con argomenti così vasti e tediosi.

Ci cimenteremo nella analisi statica del codice usando un software chiamato radare2 (disassembler), e nel debug dinamico grazie al debugger di Gnome (GDB). Terrò anche una finestra con python3 aperto per velocizzare alcune operazioni.

Il codice in questione contiene una vulnerabilità che ci permette di corrompere la memoria e di modificare una variabile, ed è il livello zero della serie Protostar di exploit education.

### Analisi Statica su Linux

Il codice si presenta in questo modo:

```c
1 int main(int argc, char **argv)
2 {
3   volatile int modified;
4   char buffer[64];
5 
6   modified = 0;
7   gets(buffer);
8 
9   if(modified != 0) {
10      printf("you have changed the 'modified' variable\n");
11  } else {
12      printf("Try again?\n");
13    }
14 }
```

In pratica abbiamo una variabile int chiamata modified ed un buffer di caratteri da 64 byte.

L’unica funzione che interagisce con le nostre variabili è la funzione gets(), che permette al programma di farci inserire dei caratteri nel buffer.

La riga che ho evidenziato nel codice [riga 9] ci mostra che l’obiettivo è mandare in input overflow il buffer in modo da cambiare il valore di “modified”.

Ti starai domandando: se non avessimo avuto il codice sorgente, come avremmo potuto individuare questa vulnerabilità senza dover eseguire il codice potenzialmente dannoso?

Ti introduco il disassembler radare2.

Radare2 è il mio tool preferito per disassemblare file eseguibili su terminale. E’ uno strumento molto complesso e difficile da comprendere le prime volte che lo si usa, ma con un paio di trucchi e comandi diventa il tuo migliore amico durante l’analisi di file binari.

Compilato il codice C con cc, eseguo il comando file per verificare il tipo di eseguibile:

```bash
file stack-zero
"stack-zero: Mach-O 64-bit executable arm64"
Sono su MacBook Air con chip m1, quindi il comando cc crea questo tipo di eseguibile. Poco male.
```

Apro il programma con radare2 in questo modo:

```bash
r2 stack-zero
```

Consulta un [cheat-sheet](https://gist.github.com/williballenthin/6857590dab3e2a6559d7) per una lista soddisfacente di comandi, io ti introdurrò quei pochi che servono per una analisi statica coi fiocchi:

```bash
# si apre un prompt di comandi con puntatore ad un indirizzo di memoria.

[0x100003ec4]>

# solitamente apre il programma all'indirizzo del main. casomai usa s per cambiare indirizzo di analisi

s @main             # salta -> indirizzo funzione main
s *0x100003ec4      # salta -> indirizzo puntato
s +10               # salta -> indirizzo attuale + 10

# voglio vedere le librerie e le funzioni importate / esportate dal programma

iE                  # info exports
ii                  # info imports
iI                  # binary file info 

# questi due comandi sono utilissimi perché ci possono fornire un'idea generale di cosa fa il programma. 
# un altro paio comandi prima di passare all'analisi dinamica 

aaa                 # analizza tutto 
pdb                 # print basic disassembly block
pdf                 # print advanced disassembly func
pdc                 # generate & print pseudo c code
v                   # apri la visual mode

# per dubbi su comandi basta scrivere un ? ed il terminale vi fornirà una lista di opzioni.
```

In questo caso grazie al comando “ii” avremo letto il seguente:

![radare2](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-960x336.png)

radare2 – il tool che preferisco per analisi statica
La funzione di indice 2 “gets” ci avrebbe rivelato la vulnerabilità di memoria del programma. Basta una ricerca su google per verificare che questa funzione prende in input da tastiera un buffer di caratteri, senza un controllo dei limiti, lasciando aperti possibili overflow e corruzioni di memoria.

Adesso che sappiamo qual è la vulnerabilità mi sembra un buon esercizio sfruttarla e studiarla grazie ad un analisi dinamica con gdb.

## Analisi Dinamica con GDB

Nella mia macchina virtuale con Parrot OS (Debian) il codice di stack-zero.c viene compilato con il seguente output:

![gdb_compile](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-2-960x359.png)

L’analisi dinamica con gdb può avere un focus sul codice C o sulle singole istruzioni macchina.

Un debugger ha moltissime funzioni. Vediamone alcune

```bash
gdb stack-zero               # debug del file stack-zero


(gdb) disassemble main       # mostra codice in assembly di main
layout next                  # apri il layout grafico  
run                          # esegui il programma 
```

GDB ha una fantastica vista grafica predefinita, che si accede con il comando “layout next”, e ci permette di avere lo schermo diviso orizzontalmente, tra istruzioni macchina e terminale di debug.

![terminale_gdb](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-3-960x518.png)
GDB debugger in visual mode

Se non hai familiarità con il linguaggio assembly non ti preoccupare, è abbastanza semplice avere un certo tipo di intuizione una volta presa la mano con questo tipo di analisi.

Uno dei comandi che eseguo prima di iniziare il debug è:

```bash
break *main
```

Per creare un punto di break alla prima istruzione della funzione main.

## Intro Stack & Assembly

GDB adesso ci permette di analizzare il blocco in modo molto elastico. In questo caso dobbiamo sfruttare una vulnerabilità dello stack, quindi ci concentreremo su quest’ultimo.

con “run” il debug inizia. Le prime istruzioni che ci spuntano sono <main> e <main+4>

![main_gdb](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-4-960x52.png)

Queste due operazioni ci spiegano molto della gestione dello stack da parte del sistema operativo. In pratica dopo queste due righe il registro di riferimento per la base dello stack sarà “x29”, nel quale viene salvato l’indirizzo in “sp” (stack pointer).

Complicato vero? ahahahah facciamo un passo indietro:

Lo stack è una sezione della memoria dedicata all’esecuzione di programmi. Nello stack il computer riserva segmenti di celle di memoria consecutive in modo da salvare temporaneamente variabili locali, chiamate a funzioni e altro.

L’accesso a queste sezioni di memoria avviene grazie ad un determinato offset dal puntatore di riferimento. Vediamo la terza e quarta istruzione:

![istruzioni](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-5-960x52.png)

Per fare in modo di eseguire queste righe dovrai scegliere uno tra i seguenti comandi

```bash
nexti               # next instruction in the block
stepi               # next step 
refresh             # comando refresh schermo (spesso dà problemi)
```

In particolare vedete due variabili che serviranno più tardi al programma. Nei registri w0 e x1 vengono salvati gli indirizzi di memoria calcolati tra le parentesi quadre.

Le istruzioni come queste sono operazioni elementari della CPU. il codice varia in base all’architettura (ARM, x86, 32/64bit…).

```bash
str w0, [sp,#28]
 
str -> salva nel registro w0
sp,#28 -> l'indirizzo calcolato come &sp + 28 
[] -> le parentesi indicano che lavoriamo con indirizzi di memoria
```

Questo tipo di accesso alla memoria è utile ed intuitivo anche per noi durante il debug, ora vedremo perché.

Sicuramente cercare di comprendere a pieno il programma solo leggendo ed eseguendo singolarmente le istruzioni assembly può risultare un operazione eccessivamente lunga e tediosa.

Quindi ci aiuteremo con dei breakpoint mirati, con un analisi attenta della memoria e con python.

## Gestione memoria e variabili

Infatti in qualsiasi momento possiamo verificare il contenuto di un registro o variabile.

```bash
x /x $sp                 # stampa in esadecimale la variabile $sp

Sintassi:
x /[numero righe][formato stampa (hex or decimal)] {$var + offset}

Esempio:
x /50x $sp               # stampa 50 celle di memoria da $sp
x /d $sp + 0x3a          # stampa il contenuto di [sp+0x3a]
```

Il comando “x” è il nostro migliore amico nella risoluzione di problemi che riguardano la memoria. Eseguendo questa riga durante il debug, noteremo che la variabile $sp punta allo stesso indirizzo durante tutta la permanenza del programma in memoria.

GDB è uno strumento molto potente, in questo caso ci aiuterà nel calcolare quante celle di memoria distano le due variabili del nostro programma: buffer e modified.

## Confronto C e Assembly

Il semplice programma stack-zero.c a cui abbiamo dato un’occhiata prima effettuava due operazioni consecutivamente:

```bash
gets(buffer);

if(modified != 0){...
```

La funzione gets salta subito all’occhio nel codice assembly, spunta come chiamata a funzione all’indirizzo <main+28>. Durante il debug sarebbe utile inserire un “break main+28”.

Secondo i nostri calcoli, qualche istruzione dopo dovrebbe esserci un confronto tra una variabile ed uno zero:

![gets_gdb](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-6-960x140.png)

La riga evidenziata <main+32> mostra che il registro w0 è ciò che cercavamo. E’ la variabile ‘modified’, e viene salvata nel registro w0 solo un’operazione prima, in questo modo possiamo vedere in che parte dello stack questa variabile è stata collocata dal computer.

``<main+28> w0 <– sp+108

La funzione gets subito prima ci chiede di inserire ‘buffer’ di 64 caratteri (o byte), che verrà salvato nello stack.

## Analisi di un Buffer Overflow con gdb

Facendo i furbi potremmo facilmente riconoscere nei segmenti di memoria un buffer di sole lettere ‘A’ -> salvata come ‘0x41’. In particolare potremmo identificare finalmente la distanza tra ‘buffer’ e ‘modified’ cercando la prima occorrenza di 0x41.

Riavviamo il programma, rimuoviamo i vecchi breakpoint e mettiamone uno subito dopo l’operazione gets.

```bash
(gdb) info break         # list all breakpoints
(gdb) del
Delete all breakpoints? (y or n) y

(gdb) b *main
(gdb) b *main+28
(gdb) run

(gdb) continue           # continua fino al break 
# richiesto input 
AAAAAAAAAAAAAAAAAAAAAAAAA 

# dopo l'input ho un break che mi permette di ispezionare gli indirizzi di memoria

(gdb) x /20x $sp         # stampa 20 blocchi dallo stack pointer
```

![stack](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-7-960x160.png)

Per visualizzare lo stack basta decidere quanti blocchi stampare partendo da $sp
Per terminare la challenge a questo punto basta un po’ di matematica:

1. Ogni blocco che inizia con 0x_____ è da 32bit (4 byte)
2. Ogni byte ha il proprio indirizzo di memoria
3. Ogni riga da 4 blocchi possiede 16 indirizzi di memoria (0x10)
4. Ho inserito 25 ‘A’
    Ogni ‘A’ occupa 1 byte -> 1 indirizzo
    La prima A è all’indirizzo 0x____efd8
    La sua distanza da $sp è 0x28 = 40
5. La variabile modified (w0) è salvata in $sp+108
    ``(gdb) x /x $sp+108
    ``Oxfffffffff01c: 0x00000000
6. La distanza tra il buffer e la variabile da modificare:
    ``python3 >>> 0xefd8 – 0xf01c = 0x44 = 68 bytes
7. Creiamo una stringa da 68 bytes:
    ``python3 >>> print(‘A’ *68)

Copiando ed incollando direttamente questa stringa andremo a sovrascrivere la memoria dello stack fino all’indirizzo subito prima della variabile da modificare.

Aggiungendo quindi una sola lettera alla fine di questa stringa e premendo invio il programma dovrebbe fare esattamente ciò che ci aspettiamo:

![buffer_overflow](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-8-960x154.png)

Magico! Diamo un’occhiata alla memoria:

![stack2](https://mcap0.altervista.org/wp-content/uploads/2022/08/image-9-960x154.png)

Nell’ultima cella in basso a destra, leggendo da sinistra verso destra sono salvati i caratteri inseriti dopo le 68 ‘A’.