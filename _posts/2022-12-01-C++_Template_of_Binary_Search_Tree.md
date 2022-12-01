---
title: Prog2 - Template of Binary Search Tree in C++
date: 2022-12-01 00:00:00 -500
categories: [Computer Science]
tags: [c++,template,bst]
image_path: /assets/
--- 

```c++
#include <iostream>

using namespace std;

//implementare classe BST template

//classe nodo

template <class T>

class Node{

public:

T key;

Node<T>* left;

Node<T>* right;

Node<T>* parent;

Node(){}

Node(T val): key(val){}

};

//classe BST

template <class T>

class BST{

Node<T>* root;

public:

BST(){root = nullptr;}

//implementare metodo INSERT

//implementare stampa albero

//implememtare metodo DISTANZA SUCCESSORE

void insert(T val);

void inOrder(){inOrder(root);};

void inOrder(Node<T>* curr);

static int counter;

};

template <class T>

int BST<T>::counter = 0;

// metodo insert

template <class T>

void BST<T>::insert(T val){

Node<T>* nodo = new Node<T>(val);

Node<T>* x= root;

Node<T>* y = nullptr;

while(x != nullptr) {

y = x;

if (y->key < val) x = x->right;

else if(y->key == val){ //BILANCIAMENTO DUPLICATI

counter++; // aumenta di uno la variabile statica counter ogni volta che si inserisce un duplicato

//orchestra l'inserimento nel sottoalbero destro o sinistro in modo alternato

if(counter%2 == 0) x = x->right;

else x = x->left;

}

else x = x->left;

}

nodo->parent = y;

if(y == nullptr) this->root = nodo;

else if(y->key < val) y->right = nodo;

else y->left = nodo;

}

//metodo inorder

template <class T>

void BST<T>::inOrder(Node<T>* curr) {

if(curr==nullptr) return;

inOrder(curr->left);

cout << curr->key << "\t";

inOrder(curr->right);

}

/*

// distanza successore

template <class T>

int BST<T>::distanzaSuccessore(T x) {

Node<T>* nodo = search(x);

if(nodo) cout << endl << "esiste" << endl;

// il problema è scomponibile in 3 casi possibili

// CASO 1: il successore esiste ed è collocato nel sottoalbero di radice 'x'

// CASO 2: il successore esiste ed è la radice del sottoalbero contenente 'x'

// CASO 3: il successore non esiste

}

*/

int main(){

cout << "PUNTO 1 | TEST metodo insert (int & char)" << endl;

BST<int> interi;

BST<char> caratteri;

interi.insert(5);

interi.insert(10);

interi.insert(9);

interi.insert(3);

caratteri.insert('f');

caratteri.insert('k');

caratteri.insert('l');

caratteri.insert('a');

// stampa albero tramite procedura inorder

caratteri.inOrder();

cout << endl;

interi.inOrder();

cout << endl << "PUNTO 2 | Bilanciamento duplicati" << endl << endl;

//idea -> nella procedura insert se VAL è un duplicato: usa una variabile statica come contatore per inserimento nel sottoalbero sinistro o destro

cout << "variabile statica BST<char>::counter: " << BST<char>::counter << endl;

caratteri.insert('k');

cout << "caratteri.insert('k'); " << endl;

cout << "variabile statica BST<char>::counter: " << BST<char>::counter << endl;

cout << endl;

cout << "variabile statica BST<int>::counter: " << BST<int>::counter << endl;

interi.insert(5);

cout << "interi.insert(5); " << endl;

cout << "variabile statica BST<int>::counter: " << BST<int>::counter << endl;

cout << endl;

interi.inOrder();

cout << endl;

caratteri.inOrder();

return 0;

}

```