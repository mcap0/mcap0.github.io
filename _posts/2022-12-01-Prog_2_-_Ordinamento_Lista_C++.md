---
title: Prog 2 - Ordinamento Lista C++
date: 2022-12-01 00:00:00 -500
categories: [Computer Science, Programmazione]
tags: [c++,ordinamento,selection,insterion]
image_path: /assets/
--- 

```cpp
#include <iostream>
#include <typeinfo>
#include <cmath>
#include <cstdlib>
#include <ctime>

using namespace std;

class Lista{
    int *ptr;
    short len;
public:
    Lista(short n):len(n){
        ptr = new int[len];
        for (int i = 0; i < len; i++){
            ptr[i] = rand()%100 + 50;
        }
    }

    ostream &stampa(ostream &os){
        for (int i = 0; i < len; i++){
            os << ptr[i] << "\t";
        }
        return os;
    }

    void bubble(){
        bool flag = 1;
        while (flag){
            flag = 0;
            for(int i = 0; i < len - 1; i++){
                if (ptr[i] > ptr[i+1]){
                    int tmp;
                    tmp = ptr[i+1];
                    ptr[i+1] = ptr[i];
                    ptr[i] = tmp;
                    flag = 1;
                }
            }   
        }
    }

    void selection(){
        int i_min = 0, scambi = 0;
        bool flag = 1;
        while(flag){
            flag = 0;
            for(int i = scambi; i < len; i++){
                if(ptr[i] < ptr[i_min]){
                    i_min = i;
                    flag = 1;
                }
            }
            int tmp = 0;
            tmp = ptr[i_min];
            ptr[i_min] = ptr[scambi];
            ptr[scambi] = tmp;
            scambi++;
        }
    }

    void insertion(){
    int key = 0;
        for(int i = 1; i < len; i++){           //parto dal 2 el. dell'array
            key = ptr[i];                       //salvo ptr[i] in key. questo sarà l'elemento da spostare
            int j = i - 1;                      // j è un contatore che parte dall'elemento precedente di i
            while(j >= 0 && ptr[j] > key){       // ripeti finchè key non è al posto giusto
                ptr[j+1] = ptr[j];              // crea spazio
                j = j - 1;                         
            }
            ptr[j+1] = key;                     //salva key nel valore j 
        }
        
    }
};

ostream &operator <<(ostream &os, Lista &obj){
    return obj.stampa(os);
}

int main(){
    srand(time(0));
    short n = 0;
    cout << "Inserisci la lunghezza della Lista" << endl;
    cin >> n;
    Lista L(n);

    cout << L;

    cout << endl << " provando l'insertion sort... " << endl;

    L.insertion();

    cout << L;

    return 0;
}
```