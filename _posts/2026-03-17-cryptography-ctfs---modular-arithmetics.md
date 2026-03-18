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
      r = b % a;  // calcola il resto tra a e b
      a = b;      // a 
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
- `p[]` -> conterrà 0 nella prima posizione, 1 nella seconda, successivamente verrà calcolato `p[k] = (p[k-2] - p[k-1]*q[k-2]) % n`. 
- `q[]` -> contiene i quozienti della divisione intera `n / x`
- `r[]` -> contiene i resti della divisione euclidea

Non è necessario rendere questi array più lungo di 3 elementi.

Per il calcolo dell'inverso modulare la procedura è la seguente:
1. Si effettua la divisione euclidea di `n` per `x`
2. Se l'ultimo resto è zero, e il resto precedente a questo è uno, `x` possiede l'inverso modulo `n`, e il suo inverso è `p[k+2]`.

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
    p[k] = p[k-2]-p[k-1]*q[k-2]; // 
    return p[k];
  }

  return 0;
}
```

In libgmp esiste una funzione specifica [mpz_gxcdext()](https://gmplib.org/manual/Number-Theoretic-Functions#index-mpz_005fgcdext). 

## Anelli e Campi


