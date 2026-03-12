---
title: "Cryptography CTFs - Basics"
date: 2026-03-12 15:32:01 +0100
categories: [Cryptography,CTF]
tags: ["crypto", "ctf", "c", "python", "gmp"]
math: true
---

```table-of-contents
```
## Introduzione

Per risolvere challenge CTF di crittografia serve mettere in pratica competenze di lettura e scrittura codice e comprensione matematica più o meno avanzata.

La mia attualmente è _meno_ avanzata, ma è un work in progress

## Encoding

Per questioni di comodità, le stesse informazioni posso essere codificate in modalità diverse. 

Nelle challenge di crittografia, è importantissimo saper manipolare stringhe e numeri e allo stesso modo conoscere e lavorare con i diversi tipi di encoding.

### Manipolazione Ascii

Il codice ascii codifica i carattere che vediamo stampati a schermo ad un numero (o codice).
https://it.wikipedia.org/wiki/ASCII

python:
```python
chr(c) # from ascii (Dec) to char (es. chr(117) -> 'u')
ord(n) # from char to ascii (Dec) (es. ord('u') -> 117)
```
c:
```c
char* stringa = "una serie di caratteri";
int ascii = stringa[0]; // conversione implicita
printf("%d\n",ascii);

//output: 117 (ascii decimale di u)
```

Sappi che questi numeri sono la rappresentazione decimale di ascii. Altre rappresentazioni sono esadecimale, binario.

Spesso avremo a che fare con file testuali che conterranno numeri. 
Una funzione per ottenere una stringa `(char*)` che contiene il file in input [Qui](https://matteocapo.it/posts/cryptography-ctfs-cheat-sheet/#readfile)

#### Conversioni Stringa-Intero

**C:**
```c
// --- Da stringa a intero ---

// Fino a 10 cifre (32-bit)
char* str_piccola = "345678";
int num_piccolo = atoi(str_piccola);

// Fino a 19 cifre (64-bit)
char* str_media = "123456789012345678";
unsigned long long int num_medio = strtoull(str_media, NULL, 10);

// 20 cifre in poi -> vai al capitolo Grandi Numeri


// --- Da intero a stringa ---

int num = 345678;
char buffer[12]; // cifre + eventuale segno + '\0'
snprintf(buffer, sizeof(buffer), "%d", num);
```

**Python:**
```python
# Nessun limite di grandezza in Python

# Da stringa a intero
testo = "123456789012345678901234567890" 
numero = int(testo)
```


### Manipolazione Esadecimale

L'esadecimale (base 16) è ovunque nelle CTF, specialmente per rappresentare dati grezzi e ciphertext.

**Python:**
```python
# Da numero a hex e viceversa
numero = 255
valore_hex = hex(numero) # '0xff'
nuovo_numero = int(valore_hex, 16) # 255

# Da stringa/bytes a hex e viceversa
testo = b"CTF"
testo_hex = testo.hex() # '435446'
testo_originale = bytes.fromhex(testo_hex) # b'CTF
```


### Manipolazione Bytes

In Python3 c'è una netta differenza tra le stringhe (`"testo"`) e i byte (`b"testo"`). La crittografia opera quasi esclusivamente sui byte.

```python
stringa = "ciao"
# Codificare in bytes
dati_bytes = stringa.encode() # b'ciao'

# Decodificare da bytes a stringa
stringa_recuperata = dati_bytes.decode() # 'ciao'
```

> Una delle librerie più utili per i CTF crypto è `pycryptodome`. Contiene due funzioni essenziali per convertire interi testi in numeri e viceversa:

```python
from Crypto.Util.number import bytes_to_long, long_to_bytes

testo = b"flag{test}"
numero = bytes_to_long(testo) # Trasforma l'intera frase in un blocco numerico
testo_orig = long_to_bytes(numero)
```


### Base64

Un encoding che trasforma qualsiasi dato binario in una stringa di testo leggibile composta da 64 caratteri (A-Z, a-z, 0-9, +, /). Riconoscibile perché spesso finisce con uno o due segni di uguale (`=` o `==`) usati come padding.

Prende l'input a blocchi di **3 byte** alla volta. Poiché 1 byte = 8 bit, un blocco intero è composto da 24 bit (3 x 8). Spezza quei 24 bit in **4 gruppi da 6 bit** ciascuno (4 x 6 = 24) e mappa ogni gruppo (6 bit possono rappresentare 64 caratteri) al suo carattere decimale corrispondente (0=A, 1=B, ..., 62=+, 63=/).

```python
import base64

dati_segreti = b"SuperSegreto"
codificato = base64.b64encode(dati_segreti) # b'U3VwZXJTZWdyZXRv'

decodificato = base64.b64decode(codificato) # b'SuperSegreto'
```


### Grandi numeri

Nella crittografia asimmetrica (come RSA) si usano numeri lunghi centinaia di cifre.

- **Python:** Python gestisce interi di grandezza infinita in modo nativo.
- **C:** I tipi base (`int`, `long long`) non bastano. Bisogna usare librerie esterne specifiche, come la **GNU Multiple Precision Arithmetic Library (GMP)**. Utilizzerò principalmente questa nei prossimi articoli e ne farò di appositi, tanto per velocità quanto per puro sport.

[Qui](https://matteocapo.it/posts/cryptography-ctfs-cheat-sheet/#install-libgmp-for-arbitrary-precision-numbers) un mio piccolo cheatsheet per libgmp su C:


## XOR

L'operatore logico bit a bit (Exclusive OR). È la base di tantissimi cifrari. In codice si rappresenta col simbolo `^`.

Proprietà matematiche fondamentali dello XOR:

XOR di un elemento con zero ritorna l'elemento stesso

$$A \oplus 0 = A$$

XOR di un elemento con sè stesso dà zero:

$$A \oplus A = 0$$

esempi:

---

$$A \oplus A \oplus B = B$$

$$A \oplus B \oplus C \oplus B \oplus C = A$$


---

proprietà di invertibilità

$$A \oplus B = C \implies C \oplus B = A$$

```python
# XOR singolo
a = 5  # 0101
b = 3  # 0011

c = a ^ b # 6 (0110)

# XOR tra bytestrings (usando pwntools)
from pwn import xor
cifrato = xor(b"flag", b"key")
```


## Server

Spesso bisogna interagire con server remoto (es. `nc indirizzo.del.server.ctf 1337`) che genera challenge dinamiche a cui devi rispondere in tempo reale.

esempio di interazione con un server usando pwntools:
```python
from pwn import *

# Connessione al server
p = remote('indirizzo.del.server.ctf', 1337)

# manda b"2" dopo la ricezione della sequenza b"> "
p.sendlineafter(b"> ", b"2")

text = p.recvuntil(b"Round")
num = int(text.split(b"was ")[1].split(b"\n")[0])
print(p.recvregex(b"flag{.*}"))
```


## Tools
- **CyberChef:** [https://gchq.github.io/CyberChef/](https://gchq.github.io/CyberChef/) 
- **Pwntools:** `pip install pwntools` 
- **PyCryptodome:** `pip install pycryptodome` 
- **SageMath:** [https://doc.sagemath.org/html/en/installation/index.html](https://doc.sagemath.org/html/en/installation/index.html)
