---
title: "Cryptography Cheat Sheet - C, Python & Sage Fundamentals"
date: 2026-03-08 16:40:53 +0100
categories: [Cryptography,CheatSheet]
tags: ["c", "python", "cryptography", "sage"]
---

```table-of-contents
```
This document is a work in progress! I will update every time I learn something new.

It will contain solution to my recurrent problems for Cryptography CTFs.

Sorry for any bad code, please reach out if you have suggestions or concerns. As this are my personal studies (and I'm almost not using AI) there WILL be errors and weird stuff. 
## C
### Write a function that Returns Multiple Variables

```c
typedef struct result{
        int x;
        int y;
} result;

result xgcd(int a, int b){
        if (b==0) return (result) {1, 0};
        result temp = xgcd(b, a%b);
        int n = a/b;
        return (result) {temp.y, temp.x-n*temp.y};
}
```

### ReadFile

>returns a char* containing the entire file as an array

```c
char* readFile(char filename[]){

        //open file
        FILE *file;
        file = fopen(filename,"r");
        if (file == NULL){
           printf("Cannot open file");
           return -1;
        }

        // get file size for memory allocation
        fseek(file, 0, SEEK_END);
        long size = ftell(file);
        fseek(file, 0, SEEK_SET);

        // allocate memory
        char *text_array = (char*)malloc(size*(sizeof(char)+1));

        // info store
        fread(text_array, 1, size, file);
        text_array[size] = '\0';
        fclose(file);
        return text_array;
}
// usage
char* file = readFile("output_legendre.txt");
```


### Install libgmp for arbitrary precision numbers

```bash
# install the library
sudo apt install libgmp-dev

# include in the .c file
#include <gmp.h>

# to compile use -lgmp
gcc file.c -o file -lgmp
```

### Use libgmp for arbitrary precision numbers

https://gmplib.org/manual/

```c
// declare numbers
mpz_t a,b;


// initialize single numbers to 0
mpz_init(a);


// initialize multiple numbers at once to 0 (last element must be NULL)
mpz_inits(a,b,NULL);


// initialize number from string (2nd is char*, 3rd is base)
mpz_init_set_str(a,"1337",10);


// set value to mpz_t from int (initialize first)
mpz_set_ui(a,1337); //unsigned long
mpz_set_si(a,1337); //signed long


// Integer Functions (remember, a+1 does not work...)

// https://gmplib.org/manual/Integer-Functions
// first is always the result
// when without _si or _ui, the function is between mpz_t, else you can use normal integers

//divisione intera
mpz_tdiv_q(res, (mpz_t) num, (mpz_t) den);
mpz_tdiv_q_ui(res, (mpz_t) num, (unsigned long) den);

mpz_powm(res,qr,exp,p); // power with module
mpz_sub_ui(a,a,1); // a = a-1
mpz_add(res,a,b); // res = a-b


// print mpz type
gmp_printf("a = %Zd\n",a);


// write functions for mpz_t (pass result as param)
void fun(mpz_t res, mpz_t a, mpz_t b){
	mpz_add(res,a,b);	
}

// write function for mpz_t and return an int
int fun(mpz_t a, mpz_t b){
	mpz_t temp;
	mpz_tdiv_q(a,b);
	//cast the mpz_t to unsigned long
	return mpz_get_ui(temp);
}
```


### Extract variable from file

> For when you have a file containing something like: "p = 1337..."

```c
char* file = readFile("path.txt");
// copy file (strtok modifies the string locally)
char* file_copy = strdup(file);
// extract single

// estraggo p dal file
char* temp_p = strtok(strtok(file_copy, "p = "),"\n");

// estraggo p
mpz_init_set_str(p,temp_p,10);
gmp_printf("%Zd\n",p); 

```


> Code for an array of values. Remember strtok removes the values (e.g.`[,] \n=`)

```c
// extract array
file_copy = strdup(file);

// file is like "ints = [1,2,3..." put string "ints" instead of "array"
char* temp_ints = strtok(file_copy, "[,] \n=");
while (strcmp(temp_ints,"array")){
//		printf("%s\n",temp_ints); // debug
	temp_ints = strtok(NULL,"[,] \n=");
}
// skippa la parola chiave ()
temp_ints = strtok(NULL,"[,] \n=");
// inizializzo ints
int i = 0;
while (atoi(temp_ints)){
	mpz_init_set_str(ints[i],temp_ints,10);
//		gmp_printf("ints[%d] = %Zd\n",i,ints[i]);
	temp_ints = strtok(NULL,"[,] \n=");
	i++;
}

```

### Chronometer

> Disclaimer: Gemini wrote this

```c
double get_time_ms(struct timespec start, struct timespec end){
        return (end.tv_sec - start.tv_sec) * 1000.0 + (end.tv_nsec - start.tv_nsec) / 1000000.0;
}

int main(){
	struct timespec start, end;
    clock_gettime(CLOCK_MONOTONIC, &start);

	//yourcode
	
	clock_gettime(CLOCK_MONOTONIC, &end);
    double tempo = get_time_ms(start, end);

    printf("Calcolato in: %.6f ms\n\n", tempo);
}
```

## Python

### Manipulation Snippets

```python
chr(c) # from ascii(Dec) to char
ord(n) # from char to ascii (Dec)

import base64
base64.b64encode(bytes) # from bytes to base64

from Crypto.Util.number import *

long_to_bytes()
bytes_to_long()

# XOR each char
new = ''.join([chr(ord(c)^13) for c in string])
```

## Sage

https://doc.sagemath.org/html/en/installation/index.html

### Install conda

```bash
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```
### Install SageMath

```bash
# remember to reboot since conda installation
conda create -n sage sage python=3.12
conda activate sage
sage
```

### Use SageMath

>Quando saprò come usarlo, sarete i primi a saperlo :)


## Numbers

### Long Primes

>https://t5k.org/lists/small/small.html

