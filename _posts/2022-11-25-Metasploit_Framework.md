---
title: Metasploit Framework.md
date: 2022-11-25 12:00:00 -500
categories: []
tags: []
--- 

#InfoSec #Framework
# Basic Commands

`msfconsole` to start

`db_status` to check the status of the database

## Help Menu

`search` to find modules

`use` to use modules

`info`

`connect` to verify possible netcat connection

`save` to save session

### Variables

`set`

`setg` to set a variable globally

`unset` to set null

`get` to get a value

`spool` to save our console output as a file

# Core

there are 5 types of modules

-   payload
-   exploit
-   auxiliary
-   NOP
-   post

## Shell Utilities

the first thing to do would be an `db_nmap -sV [ip]`

this allows to see `hosts` and `services` . Very useful

You could also try `vulns`.

# Meterpreter

## Windows

`ps` to see processes

`migrate` to migrate process

`getuid` to find informations about the user

`sysinfo` to find informations about the system

`getprivs` to see privileges

`upload` to transfer files to the victim

`run` to run a post/ module

`ipconfig` to information about intefaces

### POST

`post/windows/gather/checkvm` to see if the machine is a vm

`post/multi/recon/local_exploit_suggester` to suggest some exploits

`post/windows/menage/enable_rdp` to try forcing rdp to be available

## Linux

# Options

Parameters you will often use are:

-   **RHOSTS:** “Remote host”, the IP address of the target system. A single IP address or a network range can be set. This will support the CIDR (Classless Inter-Domain Routing) notation (/24, /16, etc.) or a network range (10.10.10.x – 10.10.10.y). You can also use a file where targets are listed, one target per line using file:/path/of/the/target_file.txt, as you can see below.

![https://tryhackme-images.s3.amazonaws.com/user-uploads/603df7900d7b6f1dff18b0bd/room-content/138a36f26c25994fcfe47e1fab085ac8.png](https://tryhackme-images.s3.amazonaws.com/user-uploads/603df7900d7b6f1dff18b0bd/room-content/138a36f26c25994fcfe47e1fab085ac8.png)

-   **RPORT:** “Remote port”, the port on the target system the vulnerable application is running on.
-   **PAYLOAD:** The payload you will use with the exploit.
-   **LHOST:** “Localhost”, the attacking machine (your AttackBox or Kali Linux) IP address.
-   **LPORT:** “Local port”, the port you will use for the reverse shell to connect back to. This is a port on your attacking machine, and you can set it to any port not used by any other application.
-   **SESSION:** Each connection established to the target system using Metasploit will have a session ID. You will use this with post-exploitation modules that will connect to the target system using an existing connection.