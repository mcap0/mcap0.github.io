---
title: Heap Binario, risoluzione esercizio d'esame Algoritmi
date: 2022-12-23 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

# Intro

UniCT Informatica L-31, Matteo Capodicasa, A.A. 2022-2023.
Il seguente esercizio è preso dal testo di un esame. Andrò a risolverlo illustrando la mia soluzione.

```
ESERCIZIO 1
(a) Si illustri la struttura dati del max-heap binario mettendola anche in relazione con la sua rappresentazione con array

(b) Si descrivano le procedure MAX-HEAPIPY e BUILD-MAX-HEAP e si illustri l'azione di BUILD-MAX-HEAP sulla
sequenza di interi [2, 4, 10, 2, 2, 11, 13, 3, 15, 3]
(c) Si descriva l'algoritmo Heapsort.
```

# Risoluzione
>In questa sezione andrò ad analizzare le domande e il metodo di risoluzione / risposta.

Nota che le seguenti risposte potrebbero contenere errori, essendo le mie personali risposte a domande di un test universitario.

---
**Comprensione Domanda `a`:**
(a) Si illustri la struttura dati del max-heap binario mettendola anche in relazione con la sua rappresentazione con array.

In questa domanda ci viene chiesto di definire la struttura del max-heap, descrivendone la derivazione (dagli alberi di ricerca binaria) e le proprietà (definizione max-heap, ordine dei nodi); inoltre indicare le relazioni con l'array che genera l'heap.

## **Risoluzione Domanda `a`:**
La struttura dati di un max-heap binario riceve i valori da inserire al suo interno attraverso un array, valori i quali vengono posti in un albero binario completo o bilanciato, albero binario nel quale tutte le foglie sono di livello `h o h-1` con `h` altezza dell'albero, e l'altezza `h` sempre uguale a `log n` con `n` numero di nodi. Un heap di `n` nodi ha sempre `n/2` foglie. 
In un max-heap la proprietà caratteristica è che la radice dell'albero ha sempre valore maggiore delle sue foglie.

Queste proprietà, in particolare l'altezza dell'albero sempre uguale al logaritmo dei suoi nodi, permette di effettuare operazioni come la ricerca con una complessità nel caso peggiore di `O(log n)`.  

Avendo un max-heap e relativo array (coda con priorità) ordinato con algoritmo heapsort, in modo tale che dato un indice ad ogni nodo dell'albero, l'indice corrispondente nell'array ha lo stesso valore, si hanno le seguenti proprietà: 
- il nodo padre di un nodo j, è il nodo j/2
- il figlio sinistro di un nodo j, è il nodo j\*2
- il figlio destro di un nodo j, è il nodo (j\*2)+1.

---
**Comprensione Domanda `b`:**
(b) Si descrivano le procedure MAX-HEAPIPY e BUILD-MAX-HEAP e si illustri l'azione di BUILD-MAX-HEAP sulla sequenza di interi [2, 4, 10, 2, 2, 11, 13, 3, 15, 3]

La domanda ci chiede di spiegare le procedure citate, per poi chiederci di svolgerle su un array di interi.

## **Risoluzione Domanda `b`:**
Dato un array, in questo caso di valori interi, le procedure che generano la struttura heap a partire dal suddetto vettore sono:
**MAX_HEAPIFY**: è la funzione che ripristina le proprietà heap di un albero, dato un indice `i`. In particolare confronta il nodo `i` con i suoi due figli e nel caso in cui uno dei figli sia maggiore del nodo padre, la funzione effettua uno swap tra essi, per poi ricorrere sul nuovo nodo massimo.
**BUILD_MAX_HEAP**: che genera un max-heap da un array in input, anche non ordinato, richiamando la funzione `MAX_HEAPIFY` in ciascun nodo che non è una foglia.

Applichiamo la funzione `BUILD_MAX_HEAP` sull'array [2, 4, 10, 2, 2, 11, 13, 3, 15, 3].

**build_max_heap** tramite un ciclo itera l'array partendo dall'elemento di indice `size_array/2` per poi terminare appena raggiunge il primo elemento. L'array in questione ha lunghezza 10, per questo si itera partendo dall'elemento di indice 5:

**i = 5:**
array pre heapify: [-1, 2, 4, 10, 2, **2**, 11, 13, 3, 15, 3]
array post heapify 1: [-1, 2, 4, 10, 2, **15**, 11, 13, 3, **2**, 3]
>c'è stato uno scambio, quindi heapify si esegue di nuovo sull'indice del nuovo massimo, in questo caso i=4, max=8, si eseguirà heapify(8).

array post heapify 2: [-1, 2, 4, 10, 2, 15, 11, 13, 3, **2**, 3]

**i = 4:**
array pre heapify: [-1, 2, 4, 10, **2**, 15, 11, 13, 3, 2, 3]
array post heapify 1:[-1, 2, 4, 10, **13**, 15, 11, **2** 3, 2, 3]
array post heapify 2:[-1, 2, 4, 10, 13, 15, 11, **2**, 3, 2, 3]

**i = 3:**
array pre heapify: [-1, 2, 4, **10**, 13, 15, 11, 2, 3, 2, 3]
array post heapify 1: [-1, 2, 4, **15**, 13, **10**, 11, 2, 3, 2, 3]
array post heapify 2: [-1, 2, 4, 15, 13, **10**, 11, 2, 3, 2, 3]

**i = 2:**
array pre heapify: [-1, 2, **4**, 15, 13, 10, 11, 2, 3, 2, 3]
array post heapify 1: [-1, 2, **15**, **4**, 13, 10, 11, 2, 3, 2, 3]
array post heapify 2: [-1, 2, 15, **11**, 13, 10, **4**, 2, 3, 2, 3]
array post heapify 3: [-1, 2, 15, 11, 13, 10, **4**, 2, 3, 2, 3]

**i = 1:**
array pre heapify: [-1, 2, 15, 11, 13, 10, 4, 2, 3, 2, 3]
array post heapify 1: [-1, **15**, **2**, 11, 13, 10, 4, 2, 3, 2, 3]
array post heapify 2: [-1, 15, **13**, 11, **2**, 10, 4, 2, 3, 2, 3]
array post heapify 3: [-1, 15, 13, 11, **3**, 10, 4, 2, **2**, 2, 3]
## Costruzione grafica MaxHeap_

				 15
		     /	     \
		  13            11
		 /  \           /  \
	    3     10       4    2
	  /  \    /
	 3    2   3

## CODICE MAX_HEAPIFY
```C++
// ripristina le prorietà dell'albero binario in modo che soddisfi le proprietà di un maxHeap
    void maxHeapify(int i){
		if (i > size_heap()) return;
        int max = i;    // indice del massimo

        // trova l'indice dell'elemento di valore massimo
        if ((left(i) <= size_heap()) && (heap[left(i)] > heap[max])){
            max = left(i);
        }
        
        if((right(i) <= size_heap()) && (heap[right(i)] > heap[max])){
            max = right(i);
        }

        // se il massimo non è i, cambia i con max e ricorri
        if (i != max){
            swap(heap[i], heap[max]);
            maxHeapify(max);
        }
    }
```

## CODICE BUILD_MAX_HEAP
```C++
void heapbuild() {

        for (int i = size_heap()/2; i > 0; i--){
            maxHeapify(i);
        }
    }
```

## Risoluzione domanda `c`:
Si descriva l'algoritmo Heapsort.
L'algoritmo heapsort ordina l'array in input usando le propreietà della struttura dati heap, in un tempo massimo di O(n\*log n).
Iniziando con un **build_max_heap**, heapsort genera un max_heap dall'array A, per poi sfruttare un ciclo che parte da un indice `i = lunghezza(array)` per **scambiare l'elemento radice** (che ha valore massimo in tutto l'heap) con **l'ultimo elemento dell'array** (una foglia). Adesso il valore massimo avrà la corretta posizione all'interno dell'array, e la foglia sarà la nuova radice dell'heap. 
Il valore massimo, adesso l'ultimo elemento dell'array, non dovrà più essere contato all'interno dell'heap, si esegue dunque `heapsize = heapsize - 1`.
Fatto questo la funzione chiama la funzione `max_heapify` sull'indice 1, in modo da ripristinare le proprietà dell'heap e ripetere il processo per l'heap di dimensione `n-1` fino a `heapsize == 2`, ottenendo un array ordinato.

