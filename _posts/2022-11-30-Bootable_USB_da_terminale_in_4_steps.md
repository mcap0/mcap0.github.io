---
title: Bootable USB da terminale in 4 steps
date: 2022-11-30 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

Ti sei mai chiesto come masterizzare un Sistema Operativo nella tua USB senza usare tool automatici come [Rufus](https://rufus.ie/it/)?

In questa guida andrò ad installare un sistema operativo a AMD64bit su una chiavetta da circa 60GB, usando solo il terminale Bash.

Questa è una guida per il terminale Linux / MacOS.

1. Apri il terminale
2. Ottieni una lista dei dischi connessi con:
	`df -H`
![[Pasted image 20221130112055.png]]
3. Una volta individuato il dispositivo, nel mio caso `disk4s1`, dobbiamo eseguire un `umount`
	`sudo umount /dev/disk4s1`
4. Andiamo adesso a masterizzare il sistema operativo vero e proprio, usando il comando `dd`
```bash
dd if=Downloads/Isos/tails-amd64-5.7.img of=/dev/disk4 bs=4m && sync
# dove 
#if= input file (il nostro file .iso o .img)
#of= output, in questo caso usiamo disk4 (non disk4s1)
#bs=4m = per ridurre il tempo d'attesa, 
#sync = non permette al comando dd di eseguire un return prima della fine della scrittura su disco
```

A questo punto dovremo espellere il dispositivo (su MacOS `diskutil eject /dev/disk4`) e avremo una chiavetta pronta ad avviare il sistema operativo installato su qualsiasi computer che lo supporta.