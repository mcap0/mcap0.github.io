---
title: "Cryptography CTFs - Modular Arithmetics"
date: 2026-03-17 23:30:21 +0100
categories: [Cryptography,CTF]
tags: ["c", "cryptography", "ctf"]
math: true
---

## Algoritmo di Eulero

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

## Algoritmo di Eulero Esteso

Fa essenzialmente due cose contemporaneamente, entrambe molto utili e ricorrenti nell'aritmetica modulare `a` e `b`:
   - trovare in modo efficiente l'inverso modulare (se `a` e `b` sono coprimi `MCD(a,b) = 1`)
   - trovare i coefficienti `x` e `y` tali che $a \cdot x + b \cdot y = MCD(a,b)$ [Identità di Bézout](https://it.wikipedia.org/wiki/Identit%C3%A0_di_B%C3%A9zout) 

[Qui](https://web.archive.org/web/20230511143526/http://www-math.ucdenver.edu/~wcherowi/courses/m5410/exeucalg.html) un deep dive dell'algoritmo. In pratica viene iterato il normale algoritmo di Euclide e vengono mantenuti registri aggiuntivi. 
- `p[]` -> conterrà 0 nella prima posizione, 1 nella seconda, verrà calcolato `p[i] = p[i-2] - p[i-1]*q[i-2]`. Non è necessario rendere questo array più lungo di 3 elementi.
- `q[]` -> contiene gli ultimi 3 quozienti (divisione intera `a / b`)
- `r[]` -> contiene gli ultimi 3 resti della divisione euclidea



