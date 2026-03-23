---
title: "Cryptography CTFs - Modular Arithmetics"
date: 2026-03-17 23:30:21 +0100
categories: [Cryptography,CTF]
tags: ["c", "cryptography", "ctf"]
math: true
---

## Algoritmo di Euclide

Algoritmo efficiente utilizzato per calcolare il massimo comune divisore tra due numeri `a` e `b`.

Algoritmo [Wikipedia](https://it.wikipedia.org/wiki/Algoritmo_di_Euclide#Codici)
```c 
int euclide(int a, int b){
  int r = 0;
  while (b != 0){
      r = a % b;  // calcola il resto tra a e b
      a = b;     
      b = r;
  }
  return a;
}
```

Oppure in [`libgmp`](https://gmplib.org/manual/Number-Theoretic-Functions): 
```c 
mpz_gcd(res,a,b) // where all variables are mpz_t 
```

L'algoritmo controlla se `b` è zero. Se lo è, `a` è il MCD (o GCD, greatest common divisor in inglese). Se non lo è, si definisce `r` resto della divisione euclidea tra `a` e `b`. Si pone `a = b`, `b = r` e si ripete il controllo dall'inizio.

> Se il GCD di due numeri è `== 1`, allora i due numero si dicono coprimi tra loro.

## Algoritmo di Euclide Esteso

Fa essenzialmente due cose contemporaneamente, entrambe molto utili e ricorrenti nell'aritmetica modulare:
   - trovare in modo efficiente l'inverso modulare (se `x` e `n` sono coprimi,`MCD(x,n) = 1`)
   - trovare i coefficienti `p` e `n` tali che $p \cdot x + s \cdot n = MCD(x,n)$ [Identità di Bézout](https://it.wikipedia.org/wiki/Identit%C3%A0_di_B%C3%A9zout) 

[Qui](https://web.archive.org/web/20230511143526/http://www-math.ucdenver.edu/~wcherowi/courses/m5410/exeucalg.html) un deep dive dell'algoritmo. In pratica viene iterato il normale algoritmo di Euclide e vengono mantenuti registri aggiuntivi. 
- `p[]` -> conterrà 0 nella prima posizione, 1 nella seconda, successivamente verrà calcolato `p[k] = p[k-2] - p[k-1]*q[k-2] `. 
- `q[]` -> contiene i quozienti della divisione intera `n / x`
- `r[]` -> contiene i resti della divisione euclidea

Non è necessario rendere questi array più lunghi di 3 elementi.

Per il calcolo dell'inverso modulare la procedura è la seguente:
1. Si effettua la divisione euclidea di `n` per `x`
2. Se l'ultimo resto è zero, e il resto precedente a questo è uno, `x` possiede l'inverso modulo `n`, e il suo inverso è `p[k+2]`.

>Nota: il codice è volutamente un misto tra pseudo codice e C. Questo perché trovo essere il miglior modo per me di comprendere la logica di un algoritmo, esente di parti necessarie al C quali la grandezza del array e ottimizzazioni varie, ma inutili alla comprensione dell'algoritmo in sé. Ha senso la riscrittura dell'algoritmo come esercizio. Ovviamente il codice qui presente non compilerà.  

```c 
int inverso_modulare_euclide(int x, int n){
  int p[], q[], r[];
  
  p[0] = 0; p[1] = 1;
  k = 0
  
  while (x != 0){ // x prenderà il valore del resto
    if (k > 1) 
      p[k] = p[k-2] - p[k-1] * q[k-2];
  
    q[k] = n / x; // quoziente
    r[k] = n % x; // resto
    n = x; // scambio n e x (come in Eulero normale per a e b)
    x = r[k]; // x diventa il resto. Se r = 0 termina il ciclo 
    k++;
  }

  if (r[k-2] == 1){ // penultimo resto (qui, r[k-1] è l'ultimo)
    p[k] = p[k-2]-p[k-1]*q[k-2]; // questo è il p[k+2] discusso prima
    // è possibile fare normalizzazione di p[k] = p[k](mod n)
    return p[k];
  }

  return 0;
}
```

In libgmp esiste una funzione specifica [mpz_gxcdext()](https://gmplib.org/manual/Number-Theoretic-Functions#index-mpz_005fgcdext).

## Aritmetica modulare

Un sistema di aritmetica degli interi, in cui ogni volta che questi raggiungono un determinato numero `n`, detto **modulo**, questi si "riavvolgono", ripartendo da zero. [Wikipedia](https://it.wikipedia.org/wiki/Aritmetica_modulare)

Si scrive 

$$a \equiv b (mod n)$$

Esempi:

$$10 \equiv 3 (mod 7)$$

$$-10 \equiv 4 (mod 7)$$

$$15 \equiv 0 (mod 5)$$

Vale la relazione fondamentale:

$$a = b + k \cdot n$$ 

Dove `k` è un numero arbitrario.

### Relazione di Congruenza

Due numeri `a` e `b` si dicono congruenti modulo `n` se

$$a \equiv b (mod n)$$ 


## Anelli e Campi

Da qui inizio a scusarmi per la matematica fatta a patatine e kinder bueno, ma la linea che c'è tra crypto ctf e teoria dei numeri a volte non è molto d'aiuto.

Pragmaticamente:
- Un insieme di interi modulo `N` con `N` non primo può essere chiamato Anello
- Un insieme di interi modulo `p` con `p` primo definisce un Campo

In un Anello possiamo sommare, sottrarre e moltiplicare. La divisione non è garantita.

In un campo, ogni numero diverso da 0 ha un suo inverso moltiplicativo, cioè un numero `s` per cui $a \cdot s \equiv 1 (mod p)$. Per questo motivo, in un campo, possiamo effettuare anche la divisione.

> Cosa si intende per "possiamo"? 
> In questo caso, vogliamo dire che per un anello, aggiungere e moltiplicare, e per un campo, anche dividere, due operatori `a` e `b`, produrrà un risultato appartenente all'anello (o campo) iniziale. (a patto che vi ricordate di fare il modulo!) 

>Esempi di uso reale sono RSA, che lavora in un anello, Diffie-Hellman e Elliptic Curve Cryptography (ECC) che lavorano in un Campo.

## Piccolo Teorema di Fermat

O [Little Fermat Theorem](https://it.wikipedia.org/wiki/Piccolo_teorema_di_Fermat). Denota proprietà curiose ed interessanti dell'esponenziazione nei Campi e permette il calcolo veloce dell'inverso modulare.

Si nota che:

$$b^{p} \equiv b (mod p)$$

$$b^{p-1}  \equiv b^{p} \cdot b^{-1} \equiv 1 (mod p)$$

$$b^{p - 2}  \equiv b^{p-1} \cdot b^{-1} \equiv b^{-1} (mod p)$$

>Nota: la seconda e la terza formula non valgono per `b = 0` o multipli di `p`

## Residui Quadratici

Un intero `x` è un residuo quadratico modulo `p` (numero primo dispari) se esiste un numero `a` tale che $a^{2} \equiv x (mod p)$

In altri termini, se numero possiede una [radice modulare](#radice-modulare) nel campo, esso è un residuo quadratico.

Non tutti gli elementi di un campo hanno una radice modulare. In pratica, $\frac{p+1}{2}$ degli elementi in un campo sono residui quadratici (includendo lo 0), e  $\frac{p-1}{2}$ sono residui non quadratici, secondo il [Criterio di Eulero](https://en.wikipedia.org/wiki/Euler%27s_criterion).

Una proprietà dei residui quadratici (chiamo con $QR$ i residui quadratici, $NQR$ i non residui quadratici)

$$QR \cdot QR = QR$$

$$QR \cdot NQR = NQR$$

$$NQR \cdot NQR = QR$$

## Simbolo di Legendre 

Il [Simbolo di Legendre](https://en.wikipedia.org/wiki/Legendre_symbol) permette di verificare che un numero `a` sia un residuo quadratico modulo `p`.

Si indica con la notazione $(\frac{a}{p})$

Il simbolo di Legendre è una funzione di `a` e `p` definita come segue:

$$(\frac{a}{p})=a^{\frac{p-1}{2}}(mod p)$$

Quest'ultimo assumerà il valore $1$ se `a` è un residuo quadratico, `-1` se `a` è un residuo non quadratico, e `0` se $a \equiv 0 (mod p)$.

```c
mpz_legendre(a,p); // (output sarà int 1, 0 o -1)
```

## Radice Modulare

Dopo aver verificato che un numero possiede una radice quadrata modulare con una singola formula grazie al simbolo di Legendre, ovviamente abbiamo bisogno di algoritmi efficienti per calcolare questa radice quadrata.

Abbiamo due casi. Il primo, molto semplice, per $p \equiv 3 (mod 4)$, mentre il secondo $p \equiv 1 (mod 4)$ richiederà l'algoritmo di Tonelli-Shanks.

### Radice modulare nel primo caso $p \equiv 3 (mod 4)$

Nei casi particolari in cui il numero primo $p \equiv 3 mod 4$ possiamo ragionare sul piccolo teorema di Fermat e sul criterio di Eulero per ricavare una formula per calcolare la radice modulare istantaneamente.

Il criterio di Eulero è pressoché equivalente al simbolo di Legendre, serve a verificare che un dato intero è un residuo quadratico modulo `p`. 

Infatti ci dice che

$$a^{\frac{p-1}{2}} \equiv 1 (mod p)$$

se `a` è un residuo quadratico.

Da qui possiamo ricavare, dopo semplici [calcoli](https://math.stackexchange.com/questions/1273690/when-p-3-pmod-4-show-that-ap1-4-pmod-p-is-a-square-root-of-a), questa formula che calcola direttamente la radice quadrata modulare che cercavamo.

$$a^{\frac{p+1}{4}}(mod p)$$

### (TS) Algoritmo di Tonelli-Shanks

Nel secondo caso viene in nostro soccorso l'algoritmo di Tonelli-Shanks, attualmente il più utilizzato per il calcolo della radice modulare, quando `p = 1 (mod 4)`.

Se $r^{2} \equiv a (mod p)$, Tonelli-Shanks calcola $r$.

>L'algoritmo Tonelli-Shanks non funziona nel caso di moduli non primi, (esempio $N = p*q$). Trovare la radice quadrata modulo numeri compositi è un problema equivalente alla fattorizzazione, ossia, un NP-Hard Problem.

L'algoritmo di Tonelli-Shanks viene usato anche per trovare le coordinate di Elliptic Curve Cryptography.

>C'è anche [questo](https://eprint.iacr.org/2020/1407.pdf) paper che sostiene di avere un algoritmo più efficiente di Tonelli-Shanks per trovare la radice modulare nello stesso caso. Non l'ho effettivamente testato.

Un esercizio di stile molto utile è tentare l'implementazione di Tonelli-Shanks seguendo [Wikipedia](https://en.wikipedia.org/wiki/Tonelli%E2%80%93Shanks_algorithm) e [Rosetta](https://rosettacode.org/wiki/Tonelli-Shanks_algorithm), usando Python che permette la gestione di numeri arbitrariamente grandi nativamente, oppure libgmp, la libreria per la Multi-Precision Arithmetics per eccellenza.

Altrimenti trovate la mia implementazione in C su questo [link](https://matteocapo.it/posts/algoritmi-crittografici/#algoritmo-di-tonelli-shanks). 

## (CRT) Chinese Remainder Theorem 

Il CRT ci dice che se conosciamo i resti $r_{i}$ di un intero $x$ che viene diviso (in divisione euclidea) per molteplici interi $N_{i}$, si può trovare in modo univoco l'intero $x$. [Wikipedia](https://en.wikipedia.org/wiki/Chinese_remainder_theorem#)

I casi d'uso per le CTF sono molteplici, l'esempio tipico è dover ricavare un messaggio `M` avendo un certo numero `k` di resti $r_{i}$ e moduli $N_{i}$ con $0 < i \leq k$. 

L'algoritmo che conviene implementare nei propri codici è la [Constructive Proof](https://en.wikipedia.org/wiki/Chinese_remainder_theorem#Existence_(constructive_proof)) che calcola il CRT per `k=2` e poi generalizza il problema a `k=k+1`.

[Qui](https://matteocapo.it/posts/algoritmi-crittografici/#chinese-remainder-theorem-crt) la mia implementazione in C con buon vecchio libgmp.

