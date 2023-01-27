---
title: Algebra Lineare - Domande e risposte per Capitolo
date: 2023-01-27 00:00:00 -500
categories: [Computer Science, Algebra]
tags: [algebra,algebra lineare, matrici]
image_path: /assets/
math: yes
---

# Algebra Lineare

Created: October 7, 2021 9:37 AM

- Quick Find 🔍
- Definizione di Endomorfismo
    
    applicazione di un insieme avente struttura in sé stesso (dove quindi a elementi dell'insieme corrispondono elementi dell'insieme stesso), che conserva tale struttura
    
- Cosa è uno spazio vettoriale?
    
    In matematica, uno spazio vettoriale è una struttura algebrica composta da: un campo, i cui elementi sono detti scalari; un insieme, i cui elementi sono detti vettori; due operazioni binarie, dette somma e moltiplicazione per scalare, caratterizzate da determinate proprietà
    
- Cos’è un insieme di generatori
    
    Nel contesto degli spazi vettoriali un sistema di generatori (o insieme di generatori) di uno spazio o di un sottospazio vettoriale è un insieme di vettori che permette di ricostruire, mediante combinazioni lineari, tutti i vettori dello spazio.
    
- Cosa si intende per base di uno spazio vettoriale?
    
    Dato un k-spazio vettoriale V si dice **base** di V un insieme di generatori linearmente indipendenti
    
- Quando due vettori sono linearmente dipendenti?
    
    due o più vettori sono linearmente dipendenti se esistono $a_1,a_2,...,a_n$(non tutti uguali a 0)
    tali che $a_1v_1\cdot...\cdot a_nv_n=0$
    
- Come si ottiene una base avendo un insieme di generatori?
    
    Grazie al metodo degli scarti successivi
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled.png)
    
- Cosa si intende per base canonica
    
    Quella base di generatori di uno spazio vettoriale formati da vettori dove l’elemento i j è uguale ad 1 e tutti gli altri elementi sono pari a 0
    
    Spiegarlo tramite un esempio
    
- Definizione di Orlato
    
    Sia M un minore di ordine h
    Si definisce orlato di M un minore di ordine h+1 contenente M
    
- Definizione Applicazioni lineari
    
    Siano U e V dei k-spazi vettoriali e f: U→V
    
    f è un’applicazione lineare se 
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%201.png)
    
- Definizione nucleo di f
    
    è l’insieme di punti che vengono annullati dalla funzione tale che 
    
    f(x) = 0; AX = 0
    
- Definizione immagine di f
    
    è l'insieme **dei** valori assunti da una funzione sul proprio dominio, ed è quindi contenuta nell'insieme **di** arrivo della funzione (il codominio), con il quale può al più coincidere
    
- Quando una variabile si dice ‘libera’
    
    Si chiamano variabili libere del sistema quelle variabili a cui, assegnando valori arbitrari, le rimanenti incognite restano determinate
    
- Qual è il numero di soluzioni i un sistema lineare?
    
    per il teorema di RC se $rk(A)\not= rk(A|B)$ nessuna soluzione
    
    altrimenti si definisce $r=rk(A)=rk(A|B)$ e $n=$ num. variabili
    
    tali che se $n=r$ si avrà una sola soluzione
    
    se $n>r$ si avranno $\infty ^{n-r}$ soluzioni
    
- Cosa dice il teorema di kronecker
    
    I sottospazi generati da una matrice hanno la stessa dimensione tra loro, che è uguale al rango della matrice A
    
    $dim_kL_r(A)=dim_kL_c(A) =rk(A)$
    
- Cosa intendiamo per ‘riduzione’ di una matrice
    
    Esiste un procedimento che ci consente di trasformare A in una matrice ridotta per righe senza alterare il rango di A
    
    Faremo operazioni del tipo $R_i = R_i+hR_j$ 
    
- Cosa sono gli elementi speciali di una matrice?
    
    Gli elementi della matrice al di sotto dei quali troviamo solo zeri sono detti ‘elementi speciali’. Ne prendiamo uno solo per riga
    
- Cosa si intende per rango di A?
    
    Si chiama rango di A l’ordine massimo di un minore non nullo della matrice
    
- Quando una matrice si dice ridotta per righe
    
    Una matrice è ridotta per righe se, in ogni riga non nulla di A, esiste un elemento non nullo al di sotto del quale vi sono tutti zeri
    
- Definizione di Minore
    
    Si chiama minore di ordine r di A il determinante di una sottomatrice quadrata di A di ordine r
    
- Cosa intendiamo per sistema lineare?
    
    Dato un campo K si dice sistema lineare un sistema di equazioni.
    
- Quando un sistema è detto di Cramer?
    
    Sia AX=B un sistema lineare
    
    Quando la matrice associata A è quadrata (numero di incognite = numero di equazioni) e il detA è diverso da 0
    
- cos’è il Determinante di una matrice?
    
    Il determinante di una matrice è quel numero associato alla matrice quadrata che ne descrive proprietà algebriche e geometriche, ed è calcolato come scalare risultato della sommatoria ...
    
- Qual è l’applicazione del Teorema di Laplace?
    
    Una serie di sviluppi ricorsivi usati per calcolare il determinante di una qualsiasi matrice quadrata
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%202.png)
    
- Enuncia e dimostra il Teorema di Cramer
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%203.png)
    
- Cosa dice il teorema di Rouche-Capelli?
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%204.png)
    
- Dimostra che se f è iniettiva → ker f = [0]
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%205.png)
    
- Dimostra che la somma di sottospazi è un sottospazio
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%206.png)
    
- Cosa dice il Lemma di Steinitz
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%207.png)
    
- Dimostrare che l’insieme di generatori L(S) è un sottospazio dello spazio vettoriale di partenza
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%208.png)
    

# Introduzione

- **definizione struttura algebrica**
    
    un insieme su cui sono definite una o più operazioni
    
- **proprietà associativa**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%209.png)
    
- **semigruppo**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2010.png)
    
- **elemento neutro**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2011.png)
    
- **dim elemento neutro**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2012.png)
    
- **Monoide**
    
    un semi gruppo in cui esiste l'elemento neutro
    
- **elemento invertibile**
    
    a * b = b * a = e
    
    e $\rarr$ a
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2013.png)
    
    ![Schermata 2021-10-05 alle 09.20.51.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-10-05_alle_09.20.51.png)
    
- **dimostrazione unicità elemento invertibile**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2014.png)
    
- **proprietà importante 2**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2015.png)
    
- **definizione gruppo**
    
    se ogni elemento è invertibile il monoide è detto gruppo
    
- **gruppi Z Q R**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2016.png)
    
- **definizione proprietà commutativa**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2017.png)
    
- **definizione gruppo abeliano**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2018.png)
    
    detto anche gruppo commutativo
    
- **Definizione Anello**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2019.png)
    
- **Dimostrazione Elemento Neutro Anello**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2020.png)
    
- **Definizione Anello con unità**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2021.png)
    
- **Esempi di anelli con unità**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2022.png)
    
- **L'elemento neutro 0 non può essere invertito**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2023.png)
    
- **Definizione Corpo**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2024.png)
    
- **Definizione Campo**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2025.png)
    
- **Esempi**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2026.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2027.png)
    
    I numeri complessi sono un altro esempio molto importante di campo
    
- **Numeri complessi cenno**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2028.png)
    
- **Elementi neutri del campo numeri complessi**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2029.png)
    
- **coppia (0,1) numeri complessi**
    
    si indica con la lettera $**i**$
    
    nota bene: $i$ è l'unico numero che elevato al quadrato ritorna un valore negativo
    
     $i^2=-1$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2030.png)
    
- **I numeri reali sono un sotto-insieme dei numeri complessi**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2031.png)
    
    un numero reale $a$ 
    può essere scritto nella forma dei numeri complessi $(a,0)$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2032.png)
    
    un numero complesso $(a,b)$  può essere scritto nella forma algebrica $a+bi$
    
- **Esercizio di esempio (come calcolare l'inverso di un numero complesso)**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2033.png)
    
    - risoluzione
        
        ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2034.png)
        

---

# Matrici

- **Definizione matrici**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2035.png)
    
- **Rappresentazione matrici**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2036.png)
    
- **Esempio rappresentazione matrici**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2037.png)
    
- **Elemento neutro**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2038.png)
    
- **Matrice opposta**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2039.png)
    

---

## O**perazioni tra matrici**

- **Somma tra matrici**
    
    *la somma è possibile solo se entrambe le matrici hanno lo stesso numero di righe e colonne*
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2040.png)
    
- **definizione scalari**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2041.png)
    
- **Prodotto tra una matrice e uno scalare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2042.png)
    
- **Proprietà del prodotto scalare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2043.png)
    
- **Prodotto tra matrici**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2044.png)
    
- **Proprietà del prodotto tra matrici**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2045.png)
    
- **AO**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2046.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2047.png)
    
- **definizione matrice quadrata**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2048.png)
    
- **Esempio prodotto**
    
    ![il prodotto in generale non è commutativo](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2049.png)
    
    il prodotto in generale non è commutativo
    
- **L'elemento neutro del prodotto**
    
    ![ un gruppo $(K^{n,n}, * ,+)$ è un anello con unità](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2050.png)
    
     un gruppo $(K^{n,n}, * ,+)$ è un anello con unità
    
- **Matrice inversa**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2051.png)
    
- **Matrice trasposta**
    
    *scambio le righe con le colonne*
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2052.png)
    
- **proprietà matrice trasposta**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2053.png)
    
- **Matrice Triangolare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2054.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2055.png)
    
- **Matrice Diagonale**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2056.png)
    
- **Matrice Scalare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2057.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2058.png)
    
- **Proprietà Matrici Diagonali**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2059.png)
    
- **Matrice simmetrica e antisimmetrica**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2060.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2061.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2062.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2063.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2064.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2065.png)
    

---

## **Insiemi**

- **Permutazioni**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2066.png)
    
- **Insieme di permutazioni su n elementi**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2067.png)
    
- **esempio permutazione**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2068.png)
    
- **Permutazione fondamentale (identità)**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2069.png)
    
- **Definizione cardinalità**
    
    *numero di elementi*
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2070.png)
    
- **Quante possibili permutazioni posso fare?**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2071.png)
    
- **Esercizio permutazioni per casa**
    
    Scrivere i 24 elementi di S$_4$ in c++ (tutte le possibili combinazioni)
    
- **Classe di una permutazione**
    
    *contare il numero di scambi per ottenere la permutazione fondamentale*
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2072.png)
    
- **Classe pari / Classe dispari**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2073.png)
    
- **Segno di una permutazione**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2074.png)
    
- **Esempio segno**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2075.png)
    

---

## **Matrici**

- **Prodotto dedotto**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2076.png)
    
- **Determinante matrice quadrata**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2077.png)
    
- **Det(A) $\R^{2,2}$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2078.png)
    
- **Det(A) $\R^{3,3}$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2079.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2080.png)
    
- **Propretà Determinante**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2081.png)
    
- **Combinazione Lineare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2082.png)
    
- **Multilinearità del determinante**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2083.png)
    

---

# Sistemi Lineari

- **Sistema lineare di m equazioni in n incognite**
    
    ![Un sistema di equazioni di questo tipo è considerato un sistema lineare](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2084.png)
    
    Un sistema di equazioni di questo tipo è considerato un sistema lineare
    
- **Incognite, coefficenti e termini noti**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2085.png)
    
- **Matrici per la risoluzione del sistema lineare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2086.png)
    
- **Sistema lineare notazione matrici**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2087.png)
    
- **Soluzione del sistema lineare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2088.png)
    
- **Sistema risolubile o impossibile**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2089.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2090.png)
    
- **Sistema di Cramer - Definizione**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2091.png)
    
- **Teorema di Cramer**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2092.png)
    
- **Esempio**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2093.png)
    
- **Esempio**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2094.png)
    

---

## Sottomatrici e Rango di una Matrice

- **Teorema di Cramer**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2095.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2096.png)
    
- **Sottomatrice**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2097.png)
    
    Esempio
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2098.png)
    
- **Minore**
    
    $r$ è il numero di righe e colonne
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%2099.png)
    
    Esempio
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20100.png)
    
- **Rango Matrice**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20101.png)
    
- **Matrici ridotte**
    
    Definizione analoga per righe e colonne
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20102.png)
    
    Esempio
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20103.png)
    
- **Elementi Speciali**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20104.png)
    
- $**rk(A)$  metodo 'matrice ridotta'**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20105.png)
    
    Esempio
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20106.png)
    
- **Metodo di Riduzione Matrice**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20107.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20108.png)
    
- **Esercizio: METODO di RIDUZIONE**
    
    ![Calcola il Rango della seguente Matrice](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20109.png)
    
    Calcola il Rango della seguente Matrice
    
    Soluzione:
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20110.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20111.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20112.png)
    

# Teorema di Rouché-Capelli

- **Notazione Matriciale Sistema Lineare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20113.png)
    
    ![Schermata 2021-11-04 alle 08.41.38.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-04_alle_08.41.38.png)
    
- **Definizione Matrice Completa**
    
    ![La matrice A non è altro che la matrice dei coefficenti delle incognite, nel sistema lineare. La matrice B è la colonna dei termini noti ](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20114.png)
    
    La matrice A non è altro che la matrice dei coefficenti delle incognite, nel sistema lineare. La matrice B è la colonna dei termini noti 
    
- **Teorema di Rouché-Capelli**
    
    ![Il rango di a solitamente è uguale al numero di incognite. ](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20115.png)
    
    Il rango di a solitamente è uguale al numero di incognite. 
    
- **Esempio**
    
    ![dal sistema lineare in basso a sinistra si ricavano C1, C2 e B. Rispettivamente, la colonna dei coefficenti della x, la colonna dei coefficenti della y e la colonna delle soluzioni. ](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20116.png)
    
    dal sistema lineare in basso a sinistra si ricavano C1, C2 e B. Rispettivamente, la colonna dei coefficenti della x, la colonna dei coefficenti della y e la colonna delle soluzioni. 
    
    ![Trovare il rango di questa matrice per confrontarlo col rango di A (= al numero di incognite non libere)](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-04_alle_08.56.25.png)
    
    Trovare il rango di questa matrice per confrontarlo col rango di A (= al numero di incognite non libere)
    
    ![Riduzione della matrice completa → risoluzione del sistema ](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20117.png)
    
    Riduzione della matrice completa → risoluzione del sistema 
    
    > Abbiamo notato che la riduzione della matrice (A|B) restituisce un sistema lineare equivalente a quello di partenza
    > 
- **Definizioni Variabili Libere**
    
    ![Schermata 2021-11-04 alle 09.19.20.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-04_alle_09.19.20.png)
    
- **Numero soluzioni in un sistema lineare**
    
    ![Schermata 2021-11-04 alle 09.20.39.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-04_alle_09.20.39.png)
    
    ![Schermata 2021-11-04 alle 09.22.07.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-04_alle_09.22.07.png)
    
    > Il sistema avrà **una sola soluzione** per $n = r$
    > 
    
    > Il sistema avrà **infinite soluzioni** per $n > r$
    > 
    
    > **Nessuna soluzione** se $rk(A) \not= rk(A|B)$
    > 
    
    quest'ultimo simbolo visualizzato male è una disuguaglianza 
    

### Esercizi: Risolvi il seguente sistema lineare

- **Esercizio 0 risolto**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20118.png)
    
    Soluzione:
    
    ![Il sistema lineare è risolubile perchè il rango delle due matrici è uguale. in questo caso però](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20119.png)
    
    Il sistema lineare è risolubile perchè il rango delle due matrici è uguale. in questo caso però
    
    > Il numero $n$  di variabili è maggiore al rango $r$ delle matrici, quindi calcolo il numero di variabili libere nel seguente modo
    > 
    
    $n = 3$              $r = 2$
    
    $n - r = 1$
    
    > Il sistema ammette $\infty^1$ equazioni
    > 
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20120.png)
    
    > In generale per la risoluzione di un sistema lineare → si calcola con il teorema di Rouchè Capelli il numero di soluzioni, infine si risolve il sistema col metodo di Cramer
    > 
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20121.png)
    
    - Scrittura più ordinata delle soluzioni
        
        ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20122.png)
        
- **Esercizio 1 con parametro da completare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20123.png)
    
    **Soluzione:**
    
    Per prima cosa scriviamo la matrice $(A|B)$
    
    ![Il prossimo passaggio è ridurre questa matrice, in modo da calcolare il rango](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20124.png)
    
    Il prossimo passaggio è ridurre questa matrice, in modo da calcolare il rango
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20125.png)
    
    Per casa: completare l'esercizio
    
- **Soluzione Esercizio 1**
    
    ![Se il determinante del quadrato giallo vale zero, il determinante dell'intera matrice è <4. Calcolo quindi per quali casi questa situazione si verifica, cioè per $h=0$  e  $h=-1$.
    Esclusi questi due casi particolari, la matrice è di $rk(A|B)=4$ → risulta maggiore del $rk(A)$, segue che il sistema lineare non presenta soluzioni per i casi $h\not=0$ e $h\not=-1$.](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20126.png)
    
    Se il determinante del quadrato giallo vale zero, il determinante dell'intera matrice è <4. Calcolo quindi per quali casi questa situazione si verifica, cioè per $h=0$  e  $h=-1$.
    Esclusi questi due casi particolari, la matrice è di $rk(A|B)=4$ → risulta maggiore del $rk(A)$, segue che il sistema lineare non presenta soluzioni per i casi $h\not=0$ e $h\not=-1$.
    
    ---
    
    **caso $h=0$**
    
    ![il sistema ammette una sola soluzione perchè → $n=r$
    ossia il numero di variabili è uguale al rango delle due matrici, segue che il numero di variabili libere è $=0$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20127.png)
    
    il sistema ammette una sola soluzione perchè → $n=r$
    ossia il numero di variabili è uguale al rango delle due matrici, segue che il numero di variabili libere è $=0$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20128.png)
    
    ---
    
    **caso $h=-1$**
    
    ![Stessa situazione di prima, il sistema ammette una sola soluzione perchè → $n=r$.](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20129.png)
    
    Stessa situazione di prima, il sistema ammette una sola soluzione perchè → $n=r$.
    
    ![$y={1\over4}$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20130.png)
    
    $y={1\over4}$
    
- **Esercizio 2 risolto**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20131.png)
    
    ---
    
    **caso $h\not=3$**
    
    ![calcolo il numero di soluzioni](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20132.png)
    
    calcolo il numero di soluzioni
    
    ![risolvo il sistema lineare ](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20133.png)
    
    risolvo il sistema lineare 
    
    ![Trovo le soluzioni e le scrivo come combinazione lineare di z](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-09_alle_09.11.29.png)
    
    Trovo le soluzioni e le scrivo come combinazione lineare di z
    
- **Esercizio 3 risolto**
    
    ![Riduco $(A|B)$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20134.png)
    
    Riduco $(A|B)$
    
    ![Calcolo  $rk(A)$ e lo confronto con $rk(A|B)$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20135.png)
    
    Calcolo  $rk(A)$ e lo confronto con $rk(A|B)$
    
    ---
    
    ![controllo per che valore del parametro $h$ l'insieme di soluzioni varia](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20136.png)
    
    controllo per che valore del parametro $h$ l'insieme di soluzioni varia
    
    ![Risolvo il sistema delle soluzioni per $h\not=$$2$ (caso generale)](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20137.png)
    
    Risolvo il sistema delle soluzioni per $h\not=$$2$ (caso generale)
    
    ---
    
    ![caso particolare $h=2$→ scelgo le variabili indipendenti (in questo caso $y$ e $t$)
    Risolvo il sistema, scrivo le soluzioni in relazione alle variabili indipendenti, scrivo le soluzioni in forma di somma di vettori
    ](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20138.png)
    
    caso particolare $h=2$→ scelgo le variabili indipendenti (in questo caso $y$ e $t$)
    Risolvo il sistema, scrivo le soluzioni in relazione alle variabili indipendenti, scrivo le soluzioni in forma di somma di vettori
    
    > Per la risoluzione di un sistema lineare 
    1. Riduco $(A|B),$  calcolo $rk(A|B)$, lo confronto con $rk(A)$ 
    2. Trovo valori del parametro che producono diversi insiemi di soluzioni
    3a. Se il sistema produce $\infty$$^n$ soluzioni → riduco e trovo un minore di variabili indipendenti; 
    3b. Se il sistema produce un numero finito di soluzioni → riduco la matrice associata 
    4. Risolvo il sistema e scrivo le soluzioni in rappresentazione: (insiemistica) (lineare)
    > 
    

## **Sottospazi Vettoriali**

- **Sottospazio Vettoriale**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20139.png)
    
- **Dimostrare Sottospazio**
    1. Dimostrare che il vettore nullo appartiene all'intersezione $o \in U\ W$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20140.png)
    
    ---
    
    1. Dimostrare che l'insieme è chiuso rispetto alle combinazioni lineari
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20141.png)
    
    ---
    
- **Somma di Sottospazi**
    
    $U+W$
    
    ![Sommiamo ogni vettore $u$ di $U$ con ogni vettore $w$ di $W$. L'insieme ottenuto è ancora un sottospazio?](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20142.png)
    
    Sommiamo ogni vettore $u$ di $U$ con ogni vettore $w$ di $W$. L'insieme ottenuto è ancora un sottospazio?
    
- **La somma di sottospazi è un sottospazio?**
    
    > Deve contenere il vettore nullo e deve essere chiuso rispetto alle combinazioni lineari
    > 
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20143.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20144.png)
    

### **Generatori di uno Spazio Vettoriale**

- $**L(S)$ combinazioni lineari**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20145.png)
    
- **Dimostrare che $L(S)$ è un sottospazio**
    
    ![Essendo $L(S)$ un sottospazio di V, esso conterrà almeno un vettore $s$, combinazione lineare di uno scalare ed $s$. Sia lo scalare $=0$ —> $0*s = 0$
    $L(S)$ contiene il vettore nullo](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20146.png)
    
    Essendo $L(S)$ un sottospazio di V, esso conterrà almeno un vettore $s$, combinazione lineare di uno scalare ed $s$. Sia lo scalare $=0$ —> $0*s = 0$
    $L(S)$ contiene il vettore nullo
    
    ---
    
    ![Le combinazioni lineari sono chiuse rispetto all'insieme](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20147.png)
    
    Le combinazioni lineari sono chiuse rispetto all'insieme
    
- **Definizione Insieme di Generatori**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20148.png)
    
- **Esempi**
    
    Caso vettore nullo:
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20149.png)
    
    ---
    
    Caso vettore singolo:
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20150.png)
    
    ---
    
    Caso vettore generico:
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20151.png)
    
    ---
    
    Caso vettore $s_2$ multiplo di $s_1$:
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20152.png)
    
    ![Schermata 2021-11-16 alle 08.57.08.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-16_alle_08.57.08.png)
    
    ---
    
- **Osservazioni particolari**
    
    ![Se calcoliamo il sottospazio di un sottospazio $U$ di un insieme $V$ , il risultato sarà lo stesso sottospazio](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20153.png)
    
    Se calcoliamo il sottospazio di un sottospazio $U$ di un insieme $V$ , il risultato sarà lo stesso sottospazio
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20154.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20155.png)
    
- **Spazio Vettoriale finitamente generato**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20156.png)
    
    ---
    
    ![$\R^2$ è generabile con solo due vettori $e_1(1,0) | e_2(0,1)$;  con $e_1, e_2$ generatori
    Il processo è applicabile ad ogni $\R^n$, con $n$ generatori.](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20157.png)
    
    $\R^2$ è generabile con solo due vettori $e_1(1,0) | e_2(0,1)$;  con $e_1, e_2$ generatori
    Il processo è applicabile ad ogni $\R^n$, con $n$ generatori.
    

## **Dipendenza Lineare**

- **Definizione Dipendenza Lineare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20158.png)
    
- **Esempi**
    
    ![L'unico vettore SINGOLO linearmente dipendente è il vettore nullo](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20159.png)
    
    L'unico vettore SINGOLO linearmente dipendente è il vettore nullo
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20160.png)
    
    ---
    
    ![Due vettori sono linearmente dipendenti solo se ←→ un vettore è multiplo dell'altro](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20161.png)
    
    Due vettori sono linearmente dipendenti solo se ←→ un vettore è multiplo dell'altro
    
    ---
    
- **Esercizio Svolto**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20162.png)
    
    ---
    
    ![Sistema lineare → se non produce soluzioni nulle → il sistema è di cramer → $det(A)=0$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20163.png)
    
    Sistema lineare → se non produce soluzioni nulle → il sistema è di cramer → $det(A)=0$
    
    ---
    
    ![Calcolo di $det(A)$ → se esistono soluzioni nell'equazione $det(A)=0$, i vettori sono linearmente dipendenti  al variare di $h\in\R$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20164.png)
    
    Calcolo di $det(A)$ → se esistono soluzioni nell'equazione $det(A)=0$, i vettori sono linearmente dipendenti  al variare di $h\in\R$
    
    Per $h=1$ → $v_1,v_2,v_3$ sono linearmente dipendenti
    
- **Criterio di indipendenza lineare**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20165.png)
    

## Base di uno Spazio Vettoriale

- **Base di uno Spazio Vettoriale**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20166.png)
    
- **Insieme Libero**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20167.png)
    
- **Proposizioni su Basi di $V$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20168.png)
    
- **DIM:  Se G (insieme finito di Generatori) allora G contiene una base**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20169.png)
    
- **Lemma di Steinitz**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20170.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20171.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20172.png)
    
- **Dimensione di $V$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20173.png)
    
- **Esercizio risolto | Generatori → Base**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20174.png)
    
    ---
    
    ![Questo è l'insieme di generatori dello spazio vettoriale $U$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20175.png)
    
    Questo è l'insieme di generatori dello spazio vettoriale $U$
    
    ---
    
    ![Algoritmo per il calcolo della base:
    1. Verifica che $u_1\not=0$ 
    2. Verifica che $u_2\notin L(u_1)$
    3. Verifica che $u_3\notin L(u_1, u_2)$
    4. Continua per tutti i vettori dell'insieme di Generatori](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20176.png)
    
    Algoritmo per il calcolo della base:
    1. Verifica che $u_1\not=0$ 
    2. Verifica che $u_2\notin L(u_1)$
    3. Verifica che $u_3\notin L(u_1, u_2)$
    4. Continua per tutti i vettori dell'insieme di Generatori
    
    ---
    
    ![In questo caso 
    1. $u_1$ si tiene perché è diverso da $0$
    2. $u_2$ non è combinazione lineare di $u_1$
    3. $u_3$ è combinazione lineare di $u_1, u_2$; in particolare, risolvendo il sistema troviamo che 
        $u_3={2\over7}u_1+{3\over7}u_2$](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20177.png)
    
    In questo caso 
    1. $u_1$ si tiene perché è diverso da $0$
    2. $u_2$ non è combinazione lineare di $u_1$
    3. $u_3$ è combinazione lineare di $u_1, u_2$; in particolare, risolvendo il sistema troviamo che 
        $u_3={2\over7}u_1+{3\over7}u_2$
    
    ---
    
    ![4. Scartiamo anche $u_4$ risolvendo il sistema](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20178.png)
    
    4. Scartiamo anche $u_4$ risolvendo il sistema
    
    ---
    
    Soluzioni 
    
    ![Abbiamo usato il metodo di scarti successivi. che restituisce la **Base** di un insieme di Generatori di partenza](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-18_alle_08.47.24.png)
    
    Abbiamo usato il metodo di scarti successivi. che restituisce la **Base** di un insieme di Generatori di partenza
    
    $$
    dim_RU=2
    $$
    
- **Determinare la dimensione di $\R$$^n$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20179.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20180.png)
    
    ---
    
    $K^{2,3}$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20181.png)
    
- **Basi Canoniche**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20182.png)
    
- **Dimostra che ogni insieme libero è estendibile a base**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20183.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20184.png)
    
- **Corollario**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20185.png)
    
    ---
    
    dim $(1)$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20186.png)
    
    ---
    
    dim $(2)$
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20187.png)
    
- **Formula di Grassman**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20188.png)
    
- **Sottospazi di $K^{m,n}$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20189.png)
    
- **Esercizio Sottospazi di $K^n$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20190.png)
    
    ---
    
    ![Calcolo il $rk(A)$ e trovo il numero di variabili Libere](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20191.png)
    
    Calcolo il $rk(A)$ e trovo il numero di variabili Libere
    
    ---
    
    ![Risolvo i sistemi lineari in funzione delle variabili non libere](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20192.png)
    
    Risolvo i sistemi lineari in funzione delle variabili non libere
    
    ---
    
    ![Scrivo le Basi in funzione delle soluzioni del sistema lineare](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20193.png)
    
    Scrivo le Basi in funzione delle soluzioni del sistema lineare
    
- **Sottospazi generati da una matrice**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20194.png)
    
- **Teorema di Kronecker**
    
    $$
    dim_kL_r(A)=dim_kL_c(A) =rk(A)
    $$
    
    > I sottospazi generati da una matrice hanno la stessa dimensione tra loro, che è uguale al rango della matrice A
    > 

## Orlati

- **Definizione Orlato**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20195.png)
    
- **Teorema degli Orlati**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20196.png)
    
- **Esercizi svolti Orlati**
    
    Esercizio 1: Determinare un sistema di equazioni di V avendo un sistema di generatori
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20197.png)
    
    ---
    
    ![Schermata 2021-11-23 alle 08.34.28.png](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Schermata_2021-11-23_alle_08.34.28.png)
    
    ---
    
    ---
    
    ---
    
    Esercizio 2: Determinare un sistema di equazioni di V avendo un sistema di generatori
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20198.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20199.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20200.png)
    
    ---
    
    ---
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20201.png)
    
- **Esercizio per casa Orlati**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20202.png)
    

# **Applicazioni Lineari**

- **Applicazioni lineari definizione**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20203.png)
    
- **Proprietà principali**
    
    ![sia $f:U\rarr V$ applicazione lineare. L'immagine del vettore nullo da $U$ in $V$  è il vettore nullo](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20204.png)
    
    sia $f:U\rarr V$ applicazione lineare. L'immagine del vettore nullo da $U$ in $V$  è il vettore nullo
    
    ---
    
    ---
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20205.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20206.png)
    
    ---
    
- **Casi Particolari**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20207.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20208.png)
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20209.png)
    
    ---
    
- **sia $f:U\rarr V$ un'applicazione lineare. Dimostra che se $f$ è iniettiva $\lrarr$ $Ker f = [o]$**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20210.png)
    
- **dim teorema**
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20211.png)
    
    ---
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20212.png)
    
- **Calcolare il $Ker f$**
    
    ![risolvere il sistema lineare omogeneo](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20213.png)
    
    risolvere il sistema lineare omogeneo
    
    ![Untitled](Algebra%20Lineare%204719661e9ae046528c7ba1485886c182/Untitled%20214.png)
    

### Esame Algebra Domande

[Exam theory and demonstrations (1)](https://www.notion.so/Exam-theory-and-demonstrations-1-852a1399fafe4f5485b7bd4be2497b67)