---
title: Python Sockets and Nmap Fundamentals
date: 2022-11-26 00:00:00 -500
categories: [Hacking from 0, Networking, Python]
tags: [networking,python,enumeration,scanning,tools,nmap]
--- 

# Sockets
## Fundamentals
### Server
```python

import socket 

# object socket             IPv4           Connection-oriented (TCP)
socketserver = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# object host & port
host = socket.gethostname()
port = 444

# binding server
socketserver.bind((host, port))

# set up a listener 
socketserver.listen(3) # max connections number

#starting the connection
while True:
	clientsocket, address = socketserver.accept() 
	print("Received connection from " % str(address))
	# 
	message = 'This is a fkin messag' +"\r\n"
	# 
	clientsocket.send(message.encode('ascii'))
	clientsocket.close()


```

### Client
```python

import socket

clientsocket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

host = socket.gethostname()
port = 444 #supposed to match with SocketServer

clientsocket.connect((host,port))

message = clientsocket.recv(1024) 

clientsocket.close()

print(message.decode('ascii'))

```

Questo script funziona solo se il client è installato in un computer esterno, e eseguito solo dopo aver inizializzato il server nel nostro PC

Ovviamente le possibilità sono infinite e oggi / nei prossimi giorni andrò a giocarci 

Se funziona solo con client python è un peso, ma se riuscissi a collegare più cose tra loro sarebbe decisamente più interessante

---

For example look at this interesting documents about [[PY Networking]]



### Fundamentals of nmap for Python
```python

import nmap

scanner = nmap.PortScanner(). #obj

ip_addr = input("Input the RHOST IP_addr: \n")

select = input("Select the type of scan: 1) __ 2) __ 3) __ \n")

if select == '1':
	print("Nmap version: ", scanner.nmap_version)
	#the actual scan
	scanner.scan(ip_addr, '1-1024', '-sV -T4 -v')
	#output the scan info
	print(scanner.scaninfo())
	#output status of the host
	print(scanner[ip_addr].state()) #Prints{up,down}
	#output open ports
	print("Open ports: ", scanner[ip_addr]['tcp'].keys())

```
example:
![[Pasted image 20220410123343.png]]
You might want to include input validation and some graphic 


# Banner Grabber
```python

import socket

s = socket.socket()
ip = input("Enter the host IP addres")
port = str(input("Enter the port"))

s.connect((ip, int(port))

s.settimeout(5) #close after 5 sec timeout

#recv = receive function 
print(s.recv(1024))

# Error handling
if s.connect_ex((ip, int(port)):
	print("connection refused")
else:
	print("port open")
```
