---
title: Web Enumeration - Guida alle Subdomain
date: 2022-12-03 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

# Intro

In questo articolo troverete tutto ciò che vi serve sapere sull'enumerazione di un sito web e i suoi sottodomini.

La enumeration consiste nell'analizzare una rete o un sistema avendo come scopo l'identificazione delle componenti vulnerabili (chiamata anche `attack surface`). In questo caso ci concentreremo nel cercare tutti i sottodomini di un web host.

# In che modo si trovano i subdomain di un sito Web

Oggi ci concentreremo su 3 strategie:
- Brute forcing
- OSINT
- Site Mapping

## Brute force con tool di enumerazione

Per il brute force di un dominio web si intende l'utilizzo di tool automatici come [GoBuster](https://mcap0.it/posts/GoBuster/), che, provvedendo una lista di parole per la ricerca dei sottodomini ci velocizzerà molto il lavoro di ricerca se il sito web ha sottodomini esposti con nomi comuni.

Se vuoi saperne di più sull'utilizzo di gobuster ti consiglio di usarlo da te stesso:

```bash
# Linux
sudo apt-get install gobuster

# Mac brew
brew install gobuster

# Gobuster Usage
gobuster dir -u http://<ip>:<port> -w <word_list_location>

#-u -> The target URL  example: https://google.com , http://10.10.10.1:80/
#-w Path to your wordlist  
#-U -P Username and Password for Basic Auth  /optional
#-c <http cookies> Specify a cookie for simulating your auth /optional
```


## OSINT Enumeration

### via Google dork

>Nella barra di ricerca di Google, inserendo alcuni operatori antecedenti al nome o al dominio del sito web, possiamo filtrare la ricerca.
- `site:.*.google.com` mostrerebbe tutti i siti web con un url come careers.google.com o cloud.google.com
- `inurl:chiave site:google.com` ci mostrerebbe invece i siti google con una particolare parola (chiave) nell'url.


### via SSL certificate

>Se il sito in questione ha un certificato SSL esistono strumenti e siti web che permettono di cercare un particolare sito web o certificato

Ad esempio https://crt.sh o https://ui.ctsearch.entrust.com/ui/ctsearchui sono database di siti web e relativi sottodomini in cui è installato un certificato, entrambi permettono di filtrare i risultati per aiutarci nelle nostre ricerche.

# Site Mapping con BurpSuite

Uno dei più completi e utili strumenti per i professionisti del web è il proxy BurpSuite, la cui installazione e funzionamento esulano dallo scope di questo articolo. Tuttavia in [questa](https://www.html.it/guide/burp-e-penetration-test-la-guida/) guida di html.it potete trovare una dettagliata introduzione sull'installazione e sulle principali funzioni di BurpSuite.

Oggi useremo BurpSuite per creare una mappa del sito target. Iniziamo aprendo un nuovo progetto, e spostandoci sulla tab: "Target".

![Pasted image 20221203114803.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020221203114803.png)
_Burp Suite ci permette di filtrare le informazioni che riceviamo dal browser in base al sito web target_

Adesso spostiamoci su Scope, e inseriamo l'url del sito di cui ci stiamo occupando:

![Pasted image 20221203115139.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020221203115139.png)

Nel browser dove è configurato il proxy possiamo adesso iniziare a cliccare un po' ovunque, e il proxy si occuperà di creare una site-map per noi.

![Pasted image 20221203115530.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020221203115530.png)

> Questo proxy può fare molto di più di quanto ho mostrato io oggi, è il non plus ultra del web-app pentesting e le sue funzioni variano dal permetterci di leggere le richieste/risposte in plain text, modificarle prima di inviarle, usare wordlist o payload, analizzare e decriptare pacchetti di rete o effettuare operazioni automatiche simili a quelle che abbiamo visto con altri tool, ma con molte più funzionalità e tutte in un unica Suite. Il punto forte di questo tool è la sua versatilità anche nella versione [Community](https://portswigger.net/burp/communitydownload) completamente gratuita.
