---
title: "Algoritmi Crittografici"
date: 2026-03-22 19:23:24 +0100
categories: [Cryptography,Codici]
tags: ["C", "libgmp", "tonellishanks"]
math: true
---

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
void tonelliShanks(mpz_t res, mpz_t n, mpz_t p){
        // check if p is prime
       if (mpz_probab_prime_p(p,PRIME_TRIES)==0) { gmp_printf("p = %Zd is definately not prime!",p); return;}

        // check if n is quadratic residue
        if (mpz_legendre(n,p)==-1) { gmp_printf("n = %Zd is not a quadratic residue\n",n); return; }

        // if p = 3 mod 4 use fermat calculation
        if(isXmodN(p,3,4)==1) { sqrtmodFermat(res,n,p); return; }

	// initialize variables
	mpz_t q,s,z,t,r,c,m,i,b,exp_temp;
        mpz_inits(q,s,z,t,r,c,m,i,b,exp_temp,NULL);

        // find Q and S such that p-1 = Q*2^S con Q dispari
	factorOddPart(q,s,p);

	// find a Z that is not a quadratic residue
	findQuadraticNonResidue(z,p);

	// t = n ** Q mod p
	mpz_powm(t,n,q,p);

	// r = n ** (q+1)/2 mod p
	mpz_set_ui(r,1);
	mpz_add(r,q,r);
	mpz_tdiv_q_ui(r,r,2);
	mpz_powm(r,n,r,p);

	// c = z**q mod p
	mpz_powm(c,z,q,p);

	// m = s
	mpz_set(m,s);

	// iterate
	while(mpz_cmp_ui(t,1)!=0){
		// if t == 0 return r = 0
		if (mpz_cmp_ui(t,0) == 0) {
			mpz_set_ui(res,0);
			mpz_clears(q, s, z, t, r, c, m, i, b, exp_temp, NULL);
			return;
		}
		// repeated squaring to find the least i < 0 < M
		repeatedSquaring(i,t,m,p);

		// b = c ** (2**(m-i-1)) mod p
		mpz_ui_pow_ui(exp_temp,2,mpz_get_ui(m)-mpz_get_ui(i)-1);
		mpz_powm(b,c,exp_temp,p);

		// r = r*b mod p
		mpz_mul(r,r,b);
		mpz_mod(r,r,p);

		// c = b**2
		mpz_powm_ui(c,b,2,p);

		// t = t * b**2
		mpz_mul(t,t,c);
		mpz_mod(t,t,p);

		// set m = i
		mpz_set(m,i);
	}
	// result is r (and -r mod p)
	mpz_set(res,r);
	mpz_clears(q, s, z, t, r, c, m, i, b, exp_temp, NULL);
}

void repeatedSquaring(mpz_t i, mpz_t t, mpz_t m, mpz_t p){
	// find the lowest i for that t ** (2**i) mod p = 1
	mpz_t res,exp;
	mpz_inits(exp,res,NULL);

	// il più piccolo i è il primo i
	for( int iter = 1; iter < mpz_get_ui(m); iter++){

		// exp = 2 ** i
		mpz_ui_pow_ui(exp,2,iter);

		// res = t ** (2 ** i) mod p
		mpz_powm(res,t,exp,p);

		// if res == 1 return iter
		if (mpz_cmp_ui(res,1)==0) {
			mpz_set_ui(i,iter);
			break;
		}
	}
	mpz_clears(exp,res,NULL);
}

void findQuadraticNonResidue(mpz_t z, mpz_t p){
	// use random approach (efficient as 50% is !qr)
	gmp_randstate_t state;
	gmp_randinit_default(state);
	gmp_randseed_ui(state,time(0));

	do{
		mpz_urandomm(z,state,p);
	} while(mpz_legendre(z,p)!=-1);

	gmp_randclear(state);
}

void factorOddPart(mpz_t q, mpz_t s, mpz_t p){
	mpz_t p_meno_uno,due;
	mpz_inits(p_meno_uno,NULL);
	mpz_init_set_ui(due,2);

	//p_meno_uno = p-1
	mpz_sub_ui(p_meno_uno,p,1);

	// find Q and S such that p-1 = Q*2^S con Q dispa
	unsigned long int temp = mpz_remove(q,p_meno_uno, due);
	mpz_set_ui(s,temp);

	mpz_clears(p_meno_uno,due,NULL);
}

int isXmodN(mpz_t val, int x, int n){
        mpz_t temp;
        mpz_init(temp);

        // temp = p mod 4
        mpz_mod_ui(temp,val,n);
        int result = (mpz_cmp_ui(temp,x)==0) ? 1 : 0;

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
if (mpz_legendre(a, p) == 1) {
  tonelliShanks(res, a, p);
  gmp_printf("%Zd is a quadratic residue!\n", a);
  gmp_printf("I tried to calculate the sqrt...\n%Zd\n", res);
}
```



