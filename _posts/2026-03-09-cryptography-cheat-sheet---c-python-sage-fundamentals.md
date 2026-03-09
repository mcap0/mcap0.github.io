---
title: "Cryptography Cheat Sheet - C, Python & Sage Fundamentals"
date: 2026-03-09 16:43:49 +0100
categories: [Appunti]
tags: []
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
void extractVarFromString(char* string_array, mpz_t num, char* srch){
        // copio la stringa (strtok modifica la stringa in locale)
        char* string_copy = strdup(string_array);
        // rimuovo caratteri dal file
        char* temp_num = strtok(string_copy, "[,] \n=");
        // cerco il nome della variabile nel file
        while(strcmp(temp_num,srch)){ temp_num = strtok(NULL,"[,] \n=");}
        // salto il nome della variabile
        temp_num = strtok(NULL,"[,] \n=");
        // inizializzo num a temp_num
        if(mpz_init_set_str(num,temp_num,10) < 0){
                printf("La variabile %s non è stata trovata nel file\n",srch);
                // debug print
//              printf("%s = %s\n",srch,temp_num);
                return;
        }
        // debug print
//      gmp_printf("%s = %Zd\n",srch,num);

}
// usage
char* file = readFile("output_legendre.txt");
mpz_t p;
mpz_init(p);
// p è il nome della variabile nel file
extractVarFromString(file,p,"p");
```


> Code for an array of values. Remember strtok removes the values (e.g.`[,] \n=`)

```c
void extractVarsFromString(char* string_array, mpz_t* nums, char* srch, int size){
    // extract array
	char* string_copy = strdup(string_array);

	// file contains smth like "ints = [1,2,3..."
	char* temp_ints = strtok(string_copy, "[,] \n=");
	// trova il nome dell'array
	while (strcmp(temp_ints,srch)){
			//debug
//          printf("%s\n",temp_ints);
			temp_ints = strtok(NULL,"[,] \n=");
	}
	// salta il nome dell'array
	temp_ints = strtok(NULL,"[,] \n=");

	// assegno i valori a nums
	int i = 0;
	while (atoi(temp_ints) && (i < size)){
			mpz_set_str(nums[i],temp_ints,10);
//          gmp_printf("nums[%d] = %Zd\n",i,nums[i]);
			temp_ints = strtok(NULL,"[,] \n=");
			i++;
	}
}

//usage
char* file = readFile("output_legendre.txt");
mpz_t ints[10];
for (int i = 0; i < 10; i++) mpz_init(ints[i]);
// p è il nome della variabile nel file
extractVarsFromString(file,ints,"ints",10);
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

