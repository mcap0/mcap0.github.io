---
title: Scansione delle Reti per CTFs.md
date: 2022-11-29 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

Spesso ci si trova in situazioni dove la chiave per completare una challenge CTF sta nel sapere dove cercare. Affinare il proprio arsenale nella scansione di rete spesso è la soluzione a problemi complessi come trovare una vulnerabilità in un Network.

Argomenti trattati:
	- Open Ports and Services - cosa sono e come funzionano
	- Come scansionare tutti i dispositivi connessi ad una rete
	- Scan completo di un computer con nmap e python

## Open Ports and Services - cosa sono e come funzionano

Quando si parla di Porte Aperte, parlando di computer, ci riferiamo alle vie di comunicazione network che un computer attiva (o apre) quando gli ordiniamo di comunicare con altri computer nella rete.

Apriamo una porta nel nostro computer (che simulerà un client) easpettiamo per un messaggio da parte di un server per comprendere meglio questo concetto:

```bash
nc -l 4444
# nc -> NetCat tool utilizzato per test di connessione, scambio di dati...
# -l -> LISTENING option 
```

>In questo modo il computer aspetterà per una connessione in entrata.

Adesso usiamo un altro computer connesso alla stessa rete. Prima di connetterci alla porta che abbiamo aperto, vediamo cosa succede se proviamo a connetterci ad una porta qualsiasi quando è chiusa.

```bash
nc 192.168.1.116 4444
(UNKNOWN) [192.168.1.116] 4444 (?) : Connection refused
```

Il nostro computer principale riceve il pacchetto di richiesta di connessione TCP generato e inviato da `nc` ([NetCat](https://it.wikipedia.org/wiki/Netcat)).
Nel caso della 'porta chiusa' la connessione verrà rifiutata.
Nel caso in cui la porta di rete del nostro computer è in attesa per una connessione (`LISTENING`) si stabilirà un canale di comunicazione. Adesso se in uno dei due computer doveste digitare del testo nel terminale e inviarlo, lo stesso testo comparirà sistematicamente nel computer dall'altro lato del canale di comunicazione. 

Il seguente è il pacchetto di rete TCP contenente 'ciao', mandato con Netcat.

![Pasted image 20221127223534.png](https://github.com/mcap0/mcap0.github.io/blob/main/assets/Pasted%20image%20221127223534.png?raw=true)
_Wireshark è un tool indispensabile nell'analisi di pacchetti di rete_

La comunicazione è completamente in 'plain text' cioè è possibile leggerla senza doverla decriptare. Inoltre in questo canale che abbiamo creato i due computer comunicano con un protocollo chiamato [TCP](https://it.wikipedia.org/wiki/Transmission_Control_Protocol). Esistono molti protocolli, spesso con scopi differenti e alcuni più sicuri di altri, problema che affronteremo in un prossimo post che spiegherà il processo di Enumerazione.

## Come scansionare tutti i dispositivi connessi ad una rete

Se un dispositivo è connesso alla tua rete, è possibile in vari modi verificarne la sua presenza.

Uno di questi è tramite `ping`, anche se dobbiamo necessariamente conoscere l'indirizzo IP della macchina a cui vogliamo connetterci.

Se vogliamo 'scovare' tutti i dispositivi connessi alla nostra rete un tool molto utile, installato di default su Kali Linux è [NetDiscover](https://www.kali.org/tools/netdiscover/).

```bash
sudo netdiscover
```

![Pasted image 20221127225042.png](https://github.com/mcap0/mcap0.github.io/blob/main/assets/Pasted%20image%20221127225042.png?raw=true)

Se vi state chiedendo come funziona questo tool, semplicemente manda un pacchetto (ARP) ad ogni indirizzo esistente in un network, segnando poi quegli IP che mandano un pacchetto di risposta. [Qui](https://www.youtube.com/watch?v=H-rANwaumfM) un video approfondimento su ARP (Address Resolution Protocol) dal canale Informaticaso su YouTube.

Avendo una lista di indirizzi IP dei computer connessi alla nostra rete adesso potremo effettuare test ad-hoc per ogni dispositivo.

Un altro metodo per scansionare automaticamente ogni dispositivo in una rete è usare [Nessus](https://www.tenable.com/products/nessus), uno dei migliori tool di scansione di rete progettato per riconoscere vulnerabilità nei sistemi informatici.

## Scan completo di un computer con nmap e python

Supponiamo adesso di sapere l'indirizzo IP di un computer che abbiamo intenzione di scansionare, ma di cui non sappiamo nient'altro. Adesso vorremmo conoscere i servizi e i protocolli che quel computer utilizza nelle sue porte aperte, in modo da poter avere un'idea di come sfruttarlo per i nostri scopi.

In questo ci può essere d'aiuto il buon vecchio [nmap](https://nmap.org)

```bash
nmap -A -p- (-T4) (-oN scan.txt) 192.168.1.1
# -A -> esegui una scansione completa
# -p- -> per tutte le porte (si può anche selezionare un range tipo 0-1000)
# -oN scan.txt -> opzionale, salva il risultato nel file scan.txt
# -T4 -> opzionale, aggiungi ottimizzazione del tempo 
```

# Conclusione
Strumenti come NetCat, Nmap e WireShark sono essenziali nella risoluzioni di problemi di rete basilari e avanzati. Possiedono una ampia quantità di funzioni, più approfondirete la conoscenza di questi strumenti e più sarà completa la vostra comprensione della rete e del suo funzionamento.
