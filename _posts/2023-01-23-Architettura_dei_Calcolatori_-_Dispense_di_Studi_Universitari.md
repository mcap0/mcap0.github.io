---
title: Architettura dei Calcolatori - Dispense di Studi Universitari
date: 2023-01-23 00:00:00 -500
categories: [Computer Science, Architettura]
tags: [assembly, computer, components]
image_path: /assets/

---
# Architettura del Calcolatori

Created: October 27, 2021 11:57 AM


# Premessa

- **che valori può assumere la tensione in un computer?**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled.png)
    
- **Perché usiamo solo due stati elettrici?**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%201.png)
    
- **quali sono i valori che associamo ad ON / OFF?**
    
    ON = 1
    
    OFF = 0
    
- **In che modo rendiamo elettronica la rappresentazione binaria?**
    
    Grazie all'algebra di Boole.
    
- **Che operazioni permette l'algebra di Boole?**
    
    somma(booleana) , prodotto(booleano), negazione
    
- **Come sommare due numeri elettronicamente?**
    
    Grazie ai circuiti logici. Partiamo dalla rappresentazione binaria, che tramite l'algebra booleana ci permette di manipolare circuiti 
    

# **Sistemi di Numerazione & Esercizi**

- **Analogie tra sistema decimale e sistema binario**
    
    Sono entrambi sistemi posizionali. a seconda della posizione la cifra assume un valore.
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%202.png)
    
- **(321)$_7$ è un numero nel sistema-7. che numero rappresenta nel sistema-10?**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%203.png)
    
- **(110101)$_2$ = (____)$_{10}$**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%204.png)
    
- **(11000010)$_2$ = (_____)$_{10}$**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%205.png)
    

# **Convertire da base-10 a base-x**

- **Convertire 13 in base-2**
    
    *dividere per 2 in ogni passaggio. calcolare il resto e scriverlo nella parte destra della colonna*
    
    *il risultato si legge dal basso verso l'alto*
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%206.png)
    
- **Convertire 13 in base-5**
    
    *stessa cosa di prima, dividere per 5 in ogni passaggio, calcolare il resto,  ...*
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%207.png)
    
- **stabilire se un numero binario è pari o dispari**
    
    leggi l'ultima cifra: 
    
    - se è 0: il numero è pari
    - se è 1: il numero è dispari
    
    ![questo numero binario è pari perchè l'ultima cifra a destra è 0.](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%208.png)
    
    questo numero binario è pari perchè l'ultima cifra a destra è 0.
    
- **tabella potenze del 2**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%209.png)
    

### Sistema Esadecimale

- **Perché introduciamo il sistema esadecimale?**
    
    Perchè ogni simbolo del sistema esadecimale rappresenta un equivalente di 4 bit.
    
    In questo modo possiamo rappresentare il sistema binario in modo compatto.
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2010.png)
    
- **Conversione da Esadecimale a Binario**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2011.png)
    

## Esempi Conversione da base 10 a base X

- **convertire 13 in base-2**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2012.png)
    
- **convertire 13 in base-5**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2013.png)
    
- **Quanti valori possono essere rappresentati dal numero P espresso in base B?**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2014.png)
    
    un numero generico avente $n$  cifre, espresso in base $B$, può rappresentare in totale:
    
    $B^n$
    
    numeri
    
- **(167)$_{10}$ = (___)$_8$**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2015.png)
    
- **(23)$_{10}$ = (___)$_2$**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2016.png)
    
- **(63)$_{10}$ = (___)$_{16}$**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2017.png)
    

### Conversione veloce da base-16 a base-2

- **Conversione da esadecimale a binario con la tabella esadecimale**
    
    Il metodo più veloce per convertire un numero esadecimale nel corrispondente numero binario prevede di usare la *tabella esadecimale*. Si tratta di una tabella che elenca i valori delle sedici cifre esadecimali in base dieci e in base due.
    
    [Tabella conversione](https://www.notion.so/1ec63c0a38fa487fbb5706ff815f3dd7)
    

## Operazioni con numeri binari

- somma
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2018.png)
    
    ![converti (ADC)$_{16}$ e (121)$_{16}$  in base-2 e somma i due numeri binari che ottieni. infine, converti in base-16 il risultato della somma](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2019.png)
    
    converti (ADC)$_{16}$ e (121)$_{16}$  in base-2 e somma i due numeri binari che ottieni. infine, converti in base-16 il risultato della somma
    

# Rappresentazione degli interi

- **Con k collegamenti elettrici ON/OFF, quante cifre possiamo rappresentare?**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2020.png)
    
    intervallo [0 : $2^k-1$]
    
- **Acronimo BIT, BYTE**
    
    **bit:** binary digit
    
    **byte:**  8 bit
    
- **WORD & DOUBLE WORD**
    
    WORD 16 bit = 2 byte
    
    DOUBLE WORD 32 bit = 4 byte
    
- **Come si rappresentano i numeri negativi?**
    
    con l'aritmetica modulare
    

## **Aritmetica modulare**

- **Intro**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2021.png)
    
- **Somma e sottrazione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2022.png)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2023.png)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2024.png)
    

### **Elementi di Aritmetica modulare**

- **Il segmento dei numeri**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2025.png)
    
- **Operazione di modulo**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2026.png)
    
- **Come rendere coerente l'aritmetica modulare**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2027.png)
    
- **Aritmetica modulare vs Aritmetica tradizionale**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2028.png)
    

## Rappresentazione in complemento a 2

- **Intervallo rappresentabile con IEEE 754**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2029.png)
    
- **con 8 bit**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2030.png)
    
- **operazioni**
    
    
- **come ottenere l'inverso (rispetto alla somma) un numero**
    
    Voglio ottenere l'inverso di 5
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2031.png)
    
    ### Assembly n * -1
    
    (5)$_{10}$ = (00000101)$_2$ 
    
    1. OPERAZIONE BOOLEANA NOT ( 1 → 0 ) ( 0 → 1 )
        
        NOT (00000101)$_2$ = 11111010
        
    2. SOMMARE 1
        
        (11111010)$_2$ + (00000001)$_2$ = 11111011
        
    
    (11111011)$_2$ = (-5)$_{10}$ 
    

# **Algebra di Boole**

- **cenni storici**
    
    E' un algebra inventata da George Boole nel 1847 con lo scopo di
    fornire un sistema formale per la logica proposizionale
    
- **sistema formale**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2032.png)
    

## **Operazioni e Proprietà**

- **Negazione (NOT)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2033.png)
    
    **proprietà**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2034.png)
    
- **Somma logica (OR)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2035.png)
    
     **proprietà**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2036.png)
    
- **Prodotto logico (AND)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2037.png)
    
    **proprietà**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2038.png)
    
- **Proprietà valide per somma e prodotto**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2039.png)
    

## **Funzioni logiche o Booleane**

- **definizione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2040.png)
    

### **TEOREMI DI DE MOGAN**

- **1**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2041.png)
    
- **2**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2042.png)
    

### Sintesi di funzioni logiche

> Come possiamo derivare una funzione analitica partendo da una tavola della verità?
> 
- **Somme di prodotti (mintermine)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2043.png)
    
- **Prodotti di somme (maxtermine)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2044.png)
    

Queste due regole assicurano la derivazione di una funzione esatta, ma non necessariamente minimizzata

### **Mappe di Karnaugh**

- **Per cosa le usiamo**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2045.png)
    

# Circuiti Logici

- **Somma Logica**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2046.png)
    
- **Negazione Logica**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2047.png)
    
- **Come combinare tra loro circuiti logici**
    
    Useremo connettori logici con circuiti elettrici molto semplici. 
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2048.png)
    
- **Esempio di funzione logica**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2049.png)
    

### **Componenti elettronici delle porte logiche**

- **Transistor MOS**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2050.png)
    
- **Transistor NMOS**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2051.png)
    
- **Transistor PMOS**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2052.png)
    
- **Inverter a MOS (NOT)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2053.png)
    
- **Porta NAND a MOS**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2054.png)
    
- **Porta AND a MOS**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2055.png)
    

## **Reti Sequenziali e combinatorie**

- **Circuiti Sequenziali**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2056.png)
    
- **Rete combinatoria - Rete sequenziale**
    
    ![Schermata 2021-10-27 alle 11.43.37.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-10-27_alle_11.43.37.png)
    
- **Flip-Flop Set-Reset**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2057.png)
    
- **FFSR Tabella**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2058.png)
    

## Implementazione ALU

- **ALU**
    
    Arithmetic-Logic-Unit
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2059.png)
    

### **Implementazione della somma binaria (4 bit)**

- **Implementazione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2060.png)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2061.png)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2062.png)
    
- **Somma ad 1 bit**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2063.png)
    
- **Half Adder**
    
    Solo per la prima colonna (a destra). L'half adder non possiede un entrata per il riporto ma genera c1, che si userà successivamente nel full adder.
    
    **n = 0**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2064.png)
    
- **Full Adder**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2065.png)
    
- **Circuito completo (4bit)**
    
    Lavoriamo con le scatole: chiudiamo il problema una volta che ce ne siamo occupati. Abbiamo visto l'architettura di un half adder e di un full adder e ora li consideriamo nel loro complessi e li rappresentiamo come delle **scatole**.
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2066.png)
    

### **Sottrazione**

- **Sottrazione in base 2**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2067.png)
    
- **4bit Adder con CarryIN**
    
    Rispetto al ciruito di somma ho solo sostituito il primo half adder con un full adder
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2068.png)
    
- **OpType**
    
    Sfrutto le proprietà del Full Adder:
    
    Uso l'ingresso carryin per dire al calcolatore se l'operazione voluta è una addizione (0) o una sottrazione (1)
    
    Questo bit è chiamato Operation Type (tipo di operazione)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2069.png)
    
- **Grazie all'OpType il circuito è universale per la somma e la sottrazione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2070.png)
    

## **Circuiti Sequenziali**

- **Flip Flop**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2071.png)
    
- **Collego il Flip Flop ad un segnale di WRITE**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2072.png)
    
- **Gated FFSR - Tabella di verità**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2073.png)
    
- **D-Type FFSR**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2074.png)
    
    In questo modo il segnale **D** viene 'mantenuto in memoria' fino ad un nuovo segnale write. Abbiamo costruito una memoria ad un bit
    
- **D-Type - Tabella**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2075.png)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2076.png)
    
- **8 bit register**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2077.png)
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2078.png)
    

# **CPU e Componenti**

- **Architettura CPU + Register File**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2079.png)
    
    ![Schermata 2021-11-03 alle 11.48.19.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-03_alle_11.48.19.png)
    
- **ALU**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2080.png)
    
- **Instruction Register (IR)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2081.png)
    
- **Instruction Decoder**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2082.png)
    
- **Program Counter (PC)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2083.png)
    
- **Bus Interface**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2084.png)
    
- **Address Lines & Address Decoder**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2085.png)
    
    ![Schermata 2021-11-03 alle 12.44.32.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-03_alle_12.44.32.png)
    
- **Control Lines**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2086.png)
    
    ![Schermata 2021-11-03 alle 12.45.52.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-03_alle_12.45.52.png)
    
- **Read**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2087.png)
    
- **Write**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2088.png)
    
- **Address Bus**
    
    > La connessione tra CPU e memoria avviene tramite il Bus di sistema il
    quale è organizzato secondo le connessioni della memoria
    > 
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2089.png)
    
- **Control Bus**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2090.png)
    
- **Data Bus**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2091.png)
    

## **Three State Logic**

- **Stati logici in uscita**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2092.png)
    
- **Terzo Stato**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2093.png)
    
- **Enable**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2094.png)
    
- **Tre stati elettrici in uscita**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2095.png)
    
- **Buffer Three State**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2096.png)
    
- **Linee Bidirezionali**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2097.png)
    
- **Esempio componente di Memoria con 2 locazioni da 1 Bit**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2098.png)
    

# Assembly

## Introduzione Memoria

- **Unità di memoria**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%2099.png)
    
- **Memory Words**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20100.png)
    
- **Organizzazione memoria**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20101.png)
    
- **Indirizzamento memoria**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20102.png)
    
- **Instruction set architecture (ISA)**
    
    I processore è in grado di eseguire un insieme di operazioni base chiamate istruzioni
    macchina
    
    L'insieme dele istruzioni eseguibili da un processore e le loro modalità d'uso è chiamato
    ISA (Instruction Set Architecture)
    
    Ogni processore commerciale ha il suo specifico ISA
    linguaggio macchina permette di definire le istruzioni attraverso un alfabeto binario {O, 1}
    
    I linguaggio assemblativo è una rappresentazione simbolica leggibile del linguaggio
    macchina
    

## **Insiemi di istruzioni RISC e CISC**

- **Reduced Instruction Set Computer (RISC)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20103.png)
    
- **Complex Instruction Set Computer (CISC)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20104.png)
    

### Istruzioni Macchina Base

> Ognuna di queste istruzioni occupa una word di memoria (da 8 a 64 bit)
> 
- **Load [destination][source]**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20105.png)
    
- **Store [source][destination]**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20106.png)
    
- **Add [destination][source1][source2]**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20107.png)
    
- **Subtract [destination][source1][source2]**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20108.png)
    
- **Esempio programma di somma**
    
    ![Schermata 2021-11-05 alle 11.39.35.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-05_alle_11.39.35.png)
    

### Modi di indirizzamento

- **Modo immediato ['#valore']**
    
    Per usare una costante numerica come operando si ricorre al modo di indirizzamento
    immediato
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20109.png)
    
- **Modo di registro ['nome indirizzo']**
    
    Il nome (= indirizzo) di un registro di processore contenentel'operando o il risultato 
    
- **Modo assoluto (diretto) ['indirizzo']**
    
    L'indirizzo di una word di memoria contenente l'operando o il
    risultato 
    
- **Modo indiretto ['indirizzo contenente indirizzo']**
    
    ![Schermata 2021-11-05 alle 11.50.26.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-05_alle_11.50.26.png)
    
- **Modo con indice o spiazzamento ['X(Ri)']**
    
    Per indicare indice e spiazzamento si usa la scrittura
    X(Ri), dove X è lo spiazzamento e Ri è nome del
    registro contenente l'indirizzo
    
- **Modo con base e indice ['(Ri, Rj)']**
    
    Esistono versioni più complesse come il modo con
    base e indice dove l'indirizzo effettivo è ottenuto
    sommando il contenuto di due registri, denotato così:
    (Ri, Rj)
    

### Salto condizionato (Branch_if)

- **Branch_if_[R2]>0 CICLO**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20110.png)
    
- **Esempio somma di N numeri con spiegazione**
    1. Carica il numero di elementi N da sommare nel registro R2
    2. Svuota il registro R3 (in questo registro andremo a sommare tutti gli elementi, quindi ci interessa che sia vuoto)
    3. Carica il prossimo numero nel reg. R5 ed esegui un [ADD R3, R3, R5]
    4. Decrementa di uno il contatore contenuto nel reg. R2
    5. Se il contatore contenuto in R2 è maggiore di zero → vai al registro chiamato CICLO
    6. Quando il contatore contenuto in R2 è uguale a zero → Salva il risultato contenuto nel registro R3 in un registro chiamato SOMMA.
    
    ![Schermata 2021-11-05 alle 12.23.40.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-05_alle_12.23.40.png)
    
- **Somma di N numeri in Assembly**
    
    ![Schermata 2021-11-05 alle 12.34.47.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-05_alle_12.34.47.png)
    

## Direttive dell'Assemblatore

- NOME **EQU** Valore_numerico
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20111.png)
    
- **ORIGIN** Indirizzo_di_memoria
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20112.png)
    
- **RESERVE** Spazio_in_byte
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20113.png)
    
- **DATAWORD** Contenuto_da_assegnare
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20114.png)
    
- **Linea di codice assemblativo**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20115.png)
    
- **Esempio**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20116.png)
    

### Accesso alla memoria

- **LDR, STR, MOV, MVN**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20117.png)
    

## Bit di Stato

- **ADD(S), SUB(S)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20118.png)
    
- **Bit di Stato**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20119.png)
    
- **Codici di condizione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20120.png)
    
- **Confronto e Salti**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20121.png)
    

# Rete Sequenziale con CLOCK

- **Reti Sequenziali**
    
    Incontreremo le stesse complicazioni delle reti combinatorie, con unica differenza la presenza di FLIP FLOP e CLOCK
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20122.png)
    
- **Bistabile Sincrono**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20123.png)
    
- **Flip Flop Master Slave**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20124.png)
    
    ![Schermata 2021-11-16 alle 08.12.01.png](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Schermata_2021-11-16_alle_08.12.01.png)
    
- **Flip Flop JK**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20125.png)
    
- **Flip Flop con Preset e Clear**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20126.png)
    

### **Registri**

- **Registri Paralleli**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20127.png)
    
- **Registri Shift (scorrimento)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20128.png)
    
- **Registro Seriale Parallelo**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20129.png)
    
- **Contatore**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20130.png)
    

# Struttura CPU

- **Classificazione Architetture CPU**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20131.png)
    
- **Esempio CISC**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20132.png)
    
- **Architettura CPU**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20133.png)
    
- **Stage-Based Architecture**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20134.png)
    
- **5 Stage Architecture (RISC)**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20135.png)
    
- Dettaglio **STR R2,[R0,#10]**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20136.png)
    

### Assembly.2

- **Istruzioni con operando immediato**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20137.png)
    
- **Istruzione di Chiamata**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20138.png)
    
- **Istruzioni con operandi in registri**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20139.png)
    
- **Scorrimento Logico**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20140.png)
    
- **Scorrimento Aritmetico**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20141.png)
    
- **Rotazione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20142.png)
    
- **LoadByte e StoreByte**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20143.png)
    
- **Moltiplicazione**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20144.png)
    
- **Divisione**
    
    Divide
    
- **Differenze RISC e CISC**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20145.png)
    
- **Operazioni CISC a due ingressi e un risultato**
    
    ![Untitled](Architettura%20del%20Calcolatori%20de1bb91b628044be8d8dd8d59cc3ece0/Untitled%20146.png)