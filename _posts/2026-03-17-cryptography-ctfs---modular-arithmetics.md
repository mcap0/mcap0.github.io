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
- `p[]` -> conterrà 0 nella prima posizione, 1 nella seconda, successivamente verrà calcolato `p[k] = p[k-2] - p[k-1]*q[k-2] (mod p)`. 
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

In un campo, ogni numero ha un suo inverso moltiplicativo, cioè un numero `s` per cui $a \cdot s \equiv 1 (mod p)$. Per questo motivo, in un campo, possiamo effettuare anche la divisione.

>Esempi di uso reale sono RSA, che lavora in un anello, Diffie-Hellman e Elliptic Curve Cryptography (ECC) che lavorano in un Campo.

##
