---
title: Esercitazione C++ Programmazione 2
date: 2022-11-26 00:00:00 -500
categories: [Computer Science, Prog2]
tags: [c++]
--- 

# Progetto
```meta
LABORATORIO DI PROGRAMMAZIONE 2
A.A.2021/2022
Lezione 02 20/04/2022
```

Dato un file contenente un elenco di utenti nel seguente formato

	ID;COGNOME;NOME;POPOLARITA
	ID;COGNOME;NOME;POPOLARITA
	ID;COGNOME;NOME;POPOLARITA

>Dove COGNOME e NOME sono stringhe e POPOLARITA è un double, implementare almeno due algoritmi di ordinamento tra quelli visti a lezione. 

Inoltre, sfruttando le librerie standard, valutarne la complessità
temporale stimando il tempo di esecuzione.

## Idea

Una Classe utenti che assegna ad ogni oggetto i propri attributi ricavati dal file utenti.txt, insieme ad un algoritmo di ordinamento, creano una lista di oggetti `utenti` ordinata in modo crescente per il parametro `popolarità`, nel quale si può portare in output ogni `User` conoscendone ID, nome e/o cognome;

### Classe utenti
```c++
class utenti{  
  
    int ID;  
    string nom, cog;  
    double pop;  
    // ottenere ID, nome e cognome dall'overloading [i] (return list[i]) dato numero array o ID  
public:  
	utenti();
    utenti(int id, string cognome, string nome, string popolarita){  
        this->ID = id;  
        this->cog = cognome;  
        this->nom = nome;  
        //conversione popolarita stringa/double  
        this->pop = stof(popolarita);  
    }  
	
	double getPop(){
	return pop;
	}
  
};

```

### Func() ordinamento

```c++
  
void insertionSort(utenti User[DIM]){  
    double key = 0;  
  
    for (int i = 1; i < DIM; i++){  
        utenti ptr(User[i]);  
        key = User[i].getPop();  
        int j = i-1;  
        while(j >= 0 && User[j].getPop() > key){  
  
            User[j+1] = User[j];  
            j--;  
  
        }  
        User[j+1] = ptr;  
    }  
}

```

### main()
```c++

int main(){  
  
    utenti Lista[DIM];  
	
    ifstream fin;  
    string temp, pop;  
    fin.open("utenti.txt");  
    while(getline(fin, temp)){  
        int l = 0;  
        int ID, i = 0;  
        string filter, cog, nom;  
        while(temp[i]!=';'){  
            filter += temp[i++];  
        }  
        ID = stoi(filter);  
        //cout << ID;     //funz  
		  
        i++;  
        filter.clear();  
        while(temp[i]!=';'){  
            filter += temp[i++];  
        }  
        cog = filter;  
		  
        i++;  
        filter.clear();  
        while(temp[i]!=';'){  
            filter += temp[i++];  
        }  
        nom = filter;  
		  
        filter.clear();  
        i++;  
        while(temp[i]) filter += temp[i++];  
        pop = filter;  
		
		utenti u(ID, cog, nom, pop);
		
        Lista[l++] = u;
		  
    }  
	
	insertionSort(Lista);  
	stampaPop(Lista);
  
}
```
