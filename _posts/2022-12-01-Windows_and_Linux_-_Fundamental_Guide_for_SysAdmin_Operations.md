---
title: Windows and Linux - Fundamental Guide for SysAdmin Operations
date: 2022-12-01 00:00:00 -500
categories: [Tech]
tags: [tech]
image_path: /assets/
--- 

# Nota

Considero la seguente sfilza di appunti una guida fondamentale in qualsiasi operazione base su Windows e Linux per quando non ci si ricorda un comando o una particolare funzione.

Questi appunti non sono interamente miei. La maggior parte di questo materiale è la trascrizione vocale del modulo Power User di Google IT Suppport Professional Certificate.

# Basic Commands

## List Directories

### PowerShell `ls`

Powershell `ls` is used to print the directories in Windows terminal. it works also in cmd.exe

To show hidden directories and system files use `ls -force`

To get help use `get-help ls -all`

Remember that in Windows the directory slash is a backslash: `\`

### Bash `ls`

to see the root `/` directory use the command

`ls /`

If you want to display hidden files too use `ls -a /[directory]`

To get help use `ls --help`

If you're a n00b use `man ls`

### Linux root Directory

`/bin`, this directory stores our essential binaries or programs. The ls command that we just used is a program, and it's located here in the `/bin` folder. It's very similar to our Windows program files directory. `/etc`, this folder stores some pretty important system configuration files. `/home`, this is the personal directory for users. It holds user documents, pictures, and etc. It's also similar to our Windows users directory. `/proc`, this directory contains information about currently running processes. We'll talk more about processes in an upcoming lesson. `/user`, the user directory doesn't actually contain our user files like our home directory. It's meant for user installed software. `/var`, we store our system logs and basically any file that constantly changes in here.

## Changing Directories

### Absolute or relative paths

One thing to call out is that there are two different types of paths, absolute and relative. An absolute path is one that starts from the main directory. A relative path is the path from your current directory.

### `pwd`

pwd works both in PS and in Bash. Stands for Print Working Directory

### `cd`

Stands for Change Directory.

To go back use `cd ..`

To go into a relative Directory use `cd ../[Dct]`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F5b4f50bd-0904-4ad0-8a93-cc33c7b4144f.png&w=3840&q=80)

## Making Directories

### PS `mkdir`

## Copying Files and Directories

### `cp`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F584e648b-48cf-4bd8-87cb-4cf66061083c.png&w=3840&q=80)

I need to be in the Dir where the file I wanna copy is.

Then i paste the Dir where I want to copy the file.

If I want to take multiple files based on a pattern I could use Wildcard: `cp *.jpg C:\Users\cindy\`

If I want to copy a directory i have to use the parameter Recurse and also Verbose

`cp '[Dir Name]' C:\[Dir]` -Recurse -Verbose

If you want to use the `~` symbol in MacOS just use the combination `options + 5`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fdfeb1165-3507-493f-86e3-0eb4f3614a22.png&w=3840&q=80)

## Moving and Renaming Files

### `mv`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F5534288e-b74e-48c5-b405-5792e0fd6ece.png&w=3840&q=80)

## Removing Files

### `rm`

## Display File Content

### `cat`

Stands for concatenate and prints the value in the terminal

in PowerShell To show a precise number of lines just add `-Head [number]` or `-Tail 10`

In Bash both `Head` and `Tail` are autonomous commands

### `open`

just opens the file.

To open in Text Editor use `open -e [Dir]`

### `PS more`

The 'more' command will get the contents of the file but will pause once it fills the terminal window. Now, we can advance the text at our own pace. When we run the 'more' command, we're launched into a separate program from the shell. This means that we interact with the more program with different keys. The Enter key advances the file by one line. You can use this if you want to move slowly through the file. Space advances the file by one page. A page in this case depends on the size of your terminal window. Basically, 'more' will output enough content to fill the terminal window. The q key allows you to quit out of 'more' and go back to your shell.

### Bash `Less`

Less does pretty much what `more` does for PowerShell

g, this moves to the beginning of a file. You can see now we're at the beginning. Capital G, this moves to the end of a text file.

`/[word]`. This allows you to search for a word or phrase. If I type in slash then type the word I want to search for, I can scan through the text file for words that match my search

## Modifying Text Files

use `open -e [FileName]` or any text editor

## Searching within Files

### PS `Select-String`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F8ecdc0cb-57c3-4261-bdb4-30705be2d19d.png&w=3840&q=80)

### Bash `grep`

`grep *.txt`

## Searching within Directories

### `ls -Filter`

## Input - Output and the Pipeline

### stdin - stdout - stderr

Every Windows process and every PowerShell command can take input and can produce output. To do this, we use something known as I/O streams or input output streams. Each process in Windows has three different streams: standard in, standard out, and standard error. It's helpful to think of these streams like actual water streams in a river. You provide input to a process by adding things to the standard in stream, which flows into the process. When the process creates output, it adds data to the standard out stream, which flows out of the process. At the CLI, the input that you provide through the keyboard goes to the standard in stream of the process that you're interacting with.

an example of stdin that gives an output is the command `echo`.

Remember that stdout is related to number 1

And stderr is related to number 2

if `rm secure_file.txt` gives us an error and we want to put this error in a file we just type the command `rm secure_file.txt 2> error.txt`

the 2 stand for stderr

If we want to send the error message to $null (the void) we just add `> $null` to the command

Get-Help about_redirection

### echo

Alias for Write-Output in PowerShell

It prints out an input.

It gives the input to a file if we use the command `echo woof > dog.txt`

It appends the input to a file if we use the command `echo woof >> dog.txt`

If the file doesn't exist, the command automatically creates a new one for us.

### Pipeline `|` and greater `>`

![Example of how to find specific words in a file combining cat command and Select-String command with the pipeline |](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F10dbff0b-236d-4340-865b-94c91bce0c98.png&w=3840&q=80)

Example of how to find specific words in a file combining cat command and Select-String command with the pipeline |

![The pipeline is used to take text and output it somewhere we want](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F69a88efa-3874-4dad-a627-95345f39606b.png&w=3840&q=80)

The pipeline is used to take text and output it somewhere we want

This Powershell Input and Output concepts all apply to Bash except for:

-   $null that in Linux is the Directory /dev/null

![Example of how to use grep to search for a specific directory or word contained in a file.](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fa1bf9a1e-601f-4703-bd3f-f3d23875c42d.png&w=3840&q=80)

Example of how to use grep to search for a specific directory or word contained in a file.

# Users, Administrators and Groups

## Users and Group Management in Windows

In Windows, by going into 'Computer Management' you can see very useful options.

In the System Tools we find something called 'Event Viewer'. We''l deep dive in this later.

The users and groups are managed by the voice 'Local Users and Groups'

![](data:image/svg+xml,%3csvg%20xmlns=%27http://www.w3.org/2000/svg%27%20version=%271.1%27%20width=%27336%27%20height=%27147.51970669110906%27/%3e)![We can put an user in the administrator group so that you don't have to be in the administrator account to use root privileges](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F97e9b255-db42-4932-b1e7-8777f6c147b9.png&w=750&q=80)

We can put an user in the administrator group so that you don't have to be in the administrator account to use root privileges

We can do these exact same things in the CLI.

### PS `Get-Local*`

`**get-localuser**` shows the users and if their account is enabled or not

`**get-localgroups**` shows the groups and their descriprions

`**get-localgroupmember [groupname]**` **shows the group members**

### Bash `sudo cat /etc/group`

in Linux we have Users, Administrators and only one root user, the first user created when installing the OS

`sudo cat /etc/group` : each line is a different group.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F41f35ba0-7102-4d34-8cb2-caff924bf2b3.png&w=3840&q=80)

We find 4 areas divided by colons.

1.  Group name
2.  Group Password (if 'x' means the passwrd is encrypted and stored somewhere else)
3.  Group ID
4.  User in the group

### Bash `sudo cat /etc/passwd`

the first user in root

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F5a1eb0a3-72d7-4692-bdfb-31b646d274dc.png&w=3840&q=80)

the first 3 fields are important

1.  User name
2.  Password (not stored here)
3.  User ID '0:0:'

## Windows Passwords

### PS `net user [user] *`

When typing this command we will change an user password. The terminal will ask us to enter the password right after this command

you could also use `net user Marco 'passwordmoltobella'` o something like this

If we want victor to change password at his next login we could run the command:

`net user victor /logonpasswordchg:yes`

### Bash `passwd [user]`

This command will ask us for the current password and then make us change it

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F8e2f4e1f-16f7-4890-8c98-2f37a9ed6586.png&w=3840&q=80)

These passwords are stored in a file called /etc/shadow

If we want to make an user password expire just use the command

`sudo passwd -e [user]`

## Adding and Removing Users

### PS `net user Andrea * /add`

the * is for the password that we're gonna set right after this command.

It's better to use the `net user victor /logonpasswordchg:yes` command when adding a new account

or we could combine them like that:

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F93e6256f-6fd8-4552-aa71-3effd0a525df.png&w=3840&q=80)

### PS `net user Andrea /del`

Or just `Remove-LocalUser Andrea`

### Bash `sudo useradd Andrea`

That will setup a default setting for a user that we could combine with `sudo passwd -e Andrea` to make him change password.

### Bash `sudo userdel Andrea`

# File Permissions

In Windows, files and directory permissions are assigned using Access Control Lists or ACLs. Specifically, we're going to work with Discretionary Access Control Lists or DACLs.

Windows files and folders can also have System Access Control Lists or SACLs assigned to them. SACLs are used to tell windows that it should use an event log to make a note of every time someone accesses a file or folder. This is a more advanced topic which you can read up on in the next supplementary reading.

You can think of a DACL as a note about who can use a file and what they're allowed to do with it. Each file or folder will have an owner and one or more DACLs.

## Permissions

**Windows**

Dir/Properties/Security

-   Read, the Read permission lets you see that a file exists, and allows you to read its contents. It also lets you read the files and directories in a directory.
-   Read and Execute, the Read and Execute permission lets you read files, and if the file is an executable, you can run the file. Read and Execute includes Read, so if you select Read and Execute, Read will automatically be selected.
-   List folder contents, List folder contents is an alias for Read and Execute on a directory. Checking one will check the other. It means that you can read and execute files in that directory.
-   Write, the Write permission lets you make changes to a file. It might be surprising to you, but you can have write access to a file without having read permission to that file. The write permission also lets you create sub directories and write two files in the directory.
-   Modify, the Modify permission is an umbrella permission that include read, execute and write.
-   Full control, a user or group with full control can do anything they want to the file.

### PS `icacls [dir]`

this command will let me see who can access a directory and what permission it has in the CLI

To change them:

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fe2eb6d8f-9811-4788-8239-10ea77391bc7.png&w=3840&q=80)

If I wanted to remove access to everyone I'd use the command

`icacls 'C:\Vacation Pictures' /remove Everyone`

To change them in cmd.exe:

use double quotes and do not use single quotes in Everyone

### Bash `ls -l [dir]`

In Linux we only have 3 permissions

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fa5e682ce-eed8-4826-9516-253deca2e80d.png&w=3840&q=80)

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fe1da57c4-3c69-49a0-9f4b-419f1e94ca4a.png&w=3840&q=80)

The first thing we see in this column is -rwxrw-r-- there are 10 bits here. The first one is the file type. In this example, dash means that the file we're looking at is just a regular file. Sometimes you might see D which stands for a directory. The next nine bits are our actual permissions, they're grouped in trios or sets of three. The first trio refers to the permission of the owner of the file. The second trio refers to the permission of the group that this file belongs to. The last trio refers to the permission of all other users. The R stands for readable, W stands for writeable and X stands for executable. Like in binary, if a bit is set then we say that it's enabled. So for our permissions, if a bit is a dash it's disabled.

To change them:

Recognize which ones permissions you want to change

![](data:image/svg+xml,%3csvg%20xmlns=%27http://www.w3.org/2000/svg%27%20version=%271.1%27%20width=%27384%27%20height=%27158.48443843031123%27/%3e)![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F680d10df-0990-4880-a230-0f199113260d.png&w=828&q=80)

### Bash `chmod` , `chown` & `chgrp`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fe5da2c84-4e4b-4e08-8f49-06f28fcc25aa.png&w=3840&q=80)

there are also numerical instances.

-   `chmod 777 file` rwxrwxrwx
-   `chmod 755 file` rwxr-xr-x
-   `chmod 644 file` rw-r—r—
-   `chmod 000 file` ————-
-   r=4 ; w=2 ; x=1

u+rwx, g+rw, o+r

**chown & chgrp**

`chown` just changes the owner of the file:

`sudo chown [user] my_cool_file`

same thing for `chgrp`

## Linux: SetUID, SetGID, Sticky Bit

Normally, if you need to change a file owned by root, you'd have to use sudo. But we want it to be able to have normal users change the files without giving them root access.

# Software and Packages Management

## Windows .exe

The concept of an executable file isn't unique to Windows, but Windows has its own special implementation of them in the form of the exe's. They're created according to Microsoft's portable executable or PE format.

They also include things like text or computer code, images that the program might use, and potentially something called an msi file. A Microsoft install package or msi is used to guide a program called the Windows Installer in the installation, maintenance, and removal of programs on the Windows operating system.

As of Windows 8, Microsoft has introduced a platform to distribute programs called the Windows Store. The Windows Store is an application repository or warehouse, where you can download and install universal Windows platform apps. Those are the applications that can run on any compatible Windows devices like desktop PCs, or tablets. These programs use a format called appx to package their contents and act like a unit of distribution.

## Linux

In this course, we'll be working with Debian packages which Ubuntu uses. A Debian package is packaged as a.deb file for Debian.

To install a Debian package, you'll need to use the D package or Debian package command. There is a standalone package here for the open source text editor, atom. Let's go ahead and install it using D package. We have to use the iFlag for the install, and that's it. Now it's installed on this computer. How about if we wanted to remove a package? To do that, we use the R or remove flag.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fb3d69a98-cd3c-42dc-9176-2b433f3d29ab.png&w=3840&q=80)

You can also list the Debian packages that are installed on your machine with a D package dash L. The L is for list.

### Bash `dpkg -l`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fd8d1f7aa-dd3f-4a4c-988d-88b2ce091a66.png&w=3840&q=80)

## Mobile App Packages

mobile operating systems use app stores. App stores are a central managed marketplace for app developers to publish and sell mobile apps. The App Store app acts like a Package Manager, and the App Store Service acts like a package repository. People use App Stores to access free and paid applications from a central source through a single interface.

But what if your organization needs to run some type of custom App? You'll need to use enterprise app management, which allows an organization to distribute custom mobile apps. These apps were developed by or for the organization, and aren't available to the general public. Enterprise apps are assigned with an enterprise certificate that has to be trusted by the devices that are installing the applications.

There's one other way to install an app into a mobile OS, and that's called side-loading. Side-loading is where you install mobile apps directly without using an App Store. Side-loading packages is riskier than installing through an App Store, and you would generally only do this if you're an app developer. Mobile apps are standalone software packages. So they contain all their dependencies. When you install an app, it will already have everything it needs to run baked in.

## Windows Archives

Popular archive types you'll see are .tar, .zip, and .rar. To install software found in an archive, you first have to extract the contents of the archive so you can see the files inside. Then, depending on what type of code it was written in, you have to use a specific method to install it.

If you're on Windows you could use the 7zip software. BUT

If you're using PowerShell version 5.0 of greater, you can actually extract and compress archives right from the command line. Let's say you've got a bunch of files in a folder called cool files on your desktop, that you'd like to add to the new zip file.

### PS `compress-archive`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Ffbbfacb7-0c56-4bce-baab-657081caf8c3.png&w=3840&q=80)

## Linux Archives

here are lots of other tools out there for archiving and unarchiving files. One tool that lots of people use, that's already installed on most Linux distros is the tar command.

### Bash tar

[http://www.linfo.org/tar.html](http://www.linfo.org/tar.html)

## Package Dependencies

Counting on other pieces of software to make an application work is called having dependencies since one bit of code depends on another in order to work.

### DLL Dynamic Link Libraries

You can think of the library as a way to package a bunch of useful code that someone else wrote. This code is bundled together into a single unit. Programs that want to use the functionality that the code provides can tap into it if they need to. In Windows, these shared libraries are called dynamic-link libraries, or DLL for short.

One super useful feature of a DLL is that the same DLL can be used by lots of different programs. This means all that shared code doesn't need to be loaded into memory for each application that wants to use it, so less memory overall is used.

### SxS side-by-side Assemblies

On modern Windows Operating Systems though, DLL hell is a problem of the past. To fix it, most shared libraries and resources in Windows are managed by something called side-by-side assemblies or SxS. Most of these shared libraries are stored in a folder at C:\Windows\WinSxS

If an application needs to use a shared library to perform a task, that library will be specified in something called a Manifest. This tells Windows to load the appropriate library from the SxS folder. The SxS system also supports access to multiple versions of the same shared library automatically.

Using a Windows package management cmdlet called Find-Package, you can locate software, along with its dependencies right from the command line.

## Package Manager

A package manager makes sure that the process of software installation, removal, update, and dependency management is as easy and automatic as possible.

But they don't do much to help you install software from a central catalog of programs or perform automatic updates. This is where a package manager like Chocolatey can come in handy. Chocolatey is a third party package manager for Windows. This means it's not written by Microsoft. It lets you install Windows applications from the command line. Chocolatey is built on some existing Windows technologies like PowerShell, and lets you install any package or software that exists in the public Chocolatey repository.

I'm currently using Homebrew as a package manager

### Choclatey

Just a refresher, the command was Find-package sysinternals include dependencies. That's all well and good. But how do we actually go about installing this package? Well, that's where the Install-Package command-let comes into play. We can use this tool to install a piece of software and its dependencies. Let's get installing that sysinternals package we found earlier shot. I'm just going to go install, package-name sysinternals. Yep, I'm just going to confirm. And just like that, we've got our package. We can verify it's in place with the Get-Package command-let. Get-Package -name sysinternals. You can also uninstall a package using the Uninstall-Package -Name sysinternals.

## Linux Packet Manager: apt

Let's see how we will install the open source graphical editor, Gimp, using APT. And if you want to follow along on your own machine, I've included a link to the Gimp download in the next reading.

### Bash `sudo apt install gimp`

Let's take a look at what this command is doing. APT grabs the dependencies that this package requires automatically and asks us if we want to install it. You can see this line here, 0 upgraded, 18 newly installed, 0 to remove, and 16 not upgraded. This gives us a good overview of what we're doing to the packages on our machine.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F1b503760-03fc-4d1f-9fc0-1bf71cd5b6ab.png&w=3840&q=80)

To remove: `sudo apt remove gimp`

If you want to get the latest package updates, you should update your package repositories with the APT update, and then, APT upgrade commands. The APT update command updates the list of packages in your repositories, so you get the latest software available. But it won't install or upgrade packages for you. Instead, once you have an updated list of packages, you can use APT upgrade, and it will install any outdated packages for you automatically.

### Linux /ect/APT/sources.list & PPA Softwares

You also noticed that when installing this package, we didn't have to download the gimp package. It was just on our system. How is that possible? Well, thanks to something known as a package repository, we don't have to manually search for each and every software we want online. We've already seen the chocolatey package repository in action. Repositories are servers that act like a central storage location for packages.

on Linux, where do you add a package or repository link? The repository source file in Ubuntu the /etc/APT/sources.list. Your computer doesn't know where to check for software if you don't explicitly add the package or repository links to this file.

If you work in a Linux environment, there are also special repositories called PPAs or personal package archives.

PPAs are hosted on Launchpad servers. Launchpad is a website owned by the organization, Canonical Limited. It allows open source software developers to develop, maintain, and distribute software. You can add PPAs like you would a regular repository link, but be a little careful when using a PPA instead of the original developer's repositories. PPA software isn't as vetted as repositories you might find from reputable sources like Ubuntu. They can sometimes contain defective, or even malicious software.

## Linux: Under the Hood

Let's see what happens in Linux when we install a Software by an Example

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fa1c490b5-3a42-40ef-aee5-bae45caf4c61.png&w=3840&q=80)

This is my Flappy app package. I've already extracted it, so you can see there's a setup script. This is a script file that will run a bunch of tasks on the computer in order to set up the package. And then flappy_app_code, this is the actual software code. The README is a pretty standard file contained in source archives that has information about the archive. It not so subtlety asks you to read it before you do anything.

A sample setup script can contain program instructions like compile flappy_app_code into machine instructions, copy compiled flappy app binary to slash bin directory, or create a folder to slash home slash, whatever the username for flappy app is. This is a very simple overview of what happens when you install a simple package. In the end, the software developers decide what their software needs to work and runs tasks to get it working.

If you knew the programming languages used, you could read these instructions yourself. But that's a bit out of scope for this course. Anyways, that's how software installation works on Linux in a nutshell.

# Device Software Management

## Windows Drivers

An important piece of software that we've talked about, but haven't really seen in action, is a driver. Remember that a driver is used to help our hardware devices interact with our operating system.

In Windows, Microsoft groups all of the devices and drivers on the computer together in a single Microsoft management console called the Device Manager.

You can get to the Device Manager in a couple of different ways. You can open up the Run dialog box and type in devmgmt.msc. Or you can right-click on This PC, select Manage, and click on the Device Manager option in the left-hand navigation menu. I'm just going to open it up from here.

When you plug a new device, like a mouse or keyboard, into your computer, the Windows operating system will go through a few steps to try and get it working. Most vendors or computer hardware manufacturers will assign a special string of characters to their devices called a hardware ID.

Once Windows has the new device's hardware ID, the OS uses it to search for the right driver for the device. It looks for it in a few places, starting with a local list of well-known drivers. Then it goes on to Windows Update or the driver store if it needs to expand the search

## Linux Devices and Drivers

Ubuntu has a slightly messier way of showing us device management. In Linux, everything's considered a file, even hardware devices. When a device is connected to your computer, a device file is created in the /dev directory

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F63efd7d3-6888-4653-b391-99920925c214.png&w=3840&q=80)

Device drivers aren't stored in the /dev directory. Sometimes, they're part of the Linux kernel. Remember, that the kernel of our machine handles the interaction with hardware. The kernel is a really monolithic piece of software that has lots of functions including support for lots of hardware. These days, a lot of hardware support is built into the kernel. So when you plug in a device, it automatically works.

But if there are devices that don't have support built into the kernel, they most likely have something called a kernel module. Well, what's this kernel module thingy? For a lot of developers, touching software like the Linux kernel is kind of intimidating. Instead, they can create kernel modules which extend the kernel's functionality without them actually touching it. So, if you need to install kernel module for a specific type of device, you can install the same way we've been installing all software in Linux.

### Block and Character devices

-   Character devices, like a keyboard or mouse, transmit data character by character.
-   Block devices like USB drives, hard drives, and CD-ROMs transfer blocks of data. A data block is just a unit of data storage.

Remember from an earlier lesson, that the first bit you see in an LS-L command is the type of file. So far, we've seen dash which stands for a regular file and a D which stands for a directory. But in this output, we can see we have a few other file types. Some of them have B for block device and C for character device.

You'll see a few files that start with /dev/sda or /sdb. SD devices are mass storage devices like our hard drives, memory sticks, et cetra. If you see an A after SD, it just means the device was detected by the computer first. So you might see something like /dev sda, /dev sdb, /dev sdc. Revisiting the /dev/null device, we can see it's considered a character device because it's used to transfer data, character by character.

## Operating System Updates

### Windows System Updates

Windows usually does a great job of telling you when there are updates to install. The Windows Update Client service runs in the background on your computer to download and install updates, and patches for your operating system. It does this by checking in with the Windows Update servers at Microsoft every so often, you can learn more in the next reading. If it finds updates that should be applied to your computer it'll download them, if you decided to allow it to, more on that later. Once the download has completed, depending on your Windows Update settings, the Windows Update Client will ask you if it's okay to install the updates or just go ahead and install them automatically. This process usually requires a restart of your computer, which the Client performs after requesting permission. In versions of Windows before Windows 10, you can tell Windows to manage your updates in a few different ways. You could have the Windows Update Client install updates and patches that Microsoft releases automatically or can let Windows Update know that you want to decide whether or not you'd like to download and install them. You can even turn off updating entirely, but that's probably not a good idea for the security reasons we talked about. You can configure Windows Update by searching updates in the search box and going to Windows Update setting.

### Linux System Updates `uname -r`

In Linux, you've already learned how to update and upgrade software on your machines. When using the apt update and apt upgrade command, they may already install security updates for you. But when you run apt upgrade, it doesn't upgrade the core operating system. In windows, our OS package is Windows 10. In Linux, It's the kernel along with other packages. The kernel controls the core components of our operating system. Like our word processors, the kernel is just another package. The kernel developers regularly include security patches, new features, and fixes for bugs in their updates. If you want to get all these things, you should be running a new kernel. To first view what kernel version you have, we going to learn a new command called uname. The uname command gives us the system information. If you use the dash r flag for kernel release, you'll see what kernel version you have

### Bash `sudo apt full-upgrade`

Before running this you should `sudo apt update` and `sudo apt upgrade`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fc4de8d47-d46d-4cfc-b44d-3d0bb2f4123c.png&w=3840&q=80)

# Filesystem

you can deep dive this subject from page 273 of the Linux Bible: CHAPTER 12

## NTFS, ext4 & FAT32

You may remember that we introduced the concept of a filesystem in the Technical Support Fundamentals course. Here's a refresher. A filesystem is used to keep track of files and file storage on a disk. Without a filesystem, the operating system wouldn't know how to organize files.

For Windows, we use the NTFS filesystem, and for Linux, it's recommended to use ext4.

They're not compatible with each other.

Luckily, there are filesystems like FAT32 that support reading and writing data to all three major operating systems. FAT32 has some shortcomings though. It doesn't support files larger than 4 gigabytes, and the size of the filesystem can't be larger than 32 gigabytes.

## Disk Anatomy

A storage disk can be divided into something called partitions. A partition is just a piece of the disk that you can manage.

When you create multiple partitions, it gives you the illusion that you're physically dividing a disk into separate disks.

### Partition and Volume distinction

Partitions essentially act as their own separate sub-disks, but they all use the same physical disk. One thing to call out is that, when you format a filesystem on a partition, it becomes a volume. Volume and partition are sometimes mistakenly used synonymously, but we want to make sure that you understand this distinction.

## Partition Tables

The other component of a disk is a partition table. A partition table tells the OS how the disk is partitioned. The table will tell you which partitions you can boot from, how much space is allocated to partition, etc. There are two main partition table schemes that are used, MBR, or Master Boot Record, and GPT, or GUID Partition Table.

### MBR: Master Boot Record

MBR is a traditional partition table, and it's mostly used in the Windows OS. MBR only lets you have volume sizes of 2 terabytes or less. It also uses something called primary partitions. You can only have four primary partitions on a disk. If you want to add more, you have to take a primary partition and make it into something known as an extended partition. Inside the extended partition, you can then make something called a logical partition. It's a little odd to get at first, but that's just how the partition table was created. This is an old standard

### GPT: GUID Partition Table

GPT is becoming the new standard for disks. You can have a volume size greater than 2 terabytes, and it only has one type of partition. You can make as many of them as you want in a disk. In an earlier lesson, we learned about a new BIOS standard called UEFI that's become the default BIOS for newer systems. To use UEFI booting, your disk has to use the GUID Partition Table.

## Disk Management

Windows actually ships with a great native of tool called the Disk Management Utility. Like most things in Windows, there are a few ways to get to disk management. We'll launch it by right clicking this PC, selecting the "Manage" option then clicking the "Disk Management" console underneath the storage grouping

BUT i want to do it from the CLI

### PS `diskpart`

`list disk` : `select disk n` : `clean` : `create partition primary` : `active`: `format FS=NTFS label=MyDrive quick` :

This last string is for 'formatting the file system to the ntfs standard, calling it MyDrive and choosing the quick setting for doing it'.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F80a03e49-806b-4848-98dd-cdd4451dde8f.png&w=3840&q=80)

### Bash `sudo parted -l`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F33fa5971-cf47-4f94-80bd-77e7bfead08c.png&w=3840&q=80)

Useful commands when disk partitioning in Linux:

-   `sudo parted /dev/sdb`
-   `print`: prints the disk informations
-   `mklabel` : gives a label to the disk
-   `mkpart primary ext4 1Mib 5GiB` : makes a partition of the currently selected disk. it's important to put the informations, the type of partition, the format style, the starting point and the end point of the disk
-   `sudo mkfs -t ext4 /dev/sdb1` : this command formatted a file system on the partition we've created

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F6df72da2-03fd-4fc3-a2c6-f226efada3b4.png&w=3840&q=80)

## Mounting Disks

### Windows

You have to mount your file system to a drive. In IT, when we refer to mounting something like a file system or a hard disk, it means that we're making something accessible to the computer. In this case, we want to make our USB drive accessible so we mount the file system to a drive. Windows does this for us automatically. You might have noticed this if you plug in a USB drive, it'll show up on your list of drives and you can start using it right away. When you're done using the drive, you'll just have to safely eject or essentially unmount the drive by right clicking and selecting eject. We'll talk about why this is important in a later lesson.

### Linux

If we create a partition to a disk but we do not mount the disk in a Linux machine we're going to see an error like this one:

![](data:image/svg+xml,%3csvg%20xmlns=%27http://www.w3.org/2000/svg%27%20version=%271.1%27%20width=%27240%27%20height=%2744.01384083044983%27/%3e)![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fe340093d-6309-4660-9856-620470463490.png&w=640&q=80)

to mount a disk we use certain commands. You can only mount a disk into a directory created before.

This directory 'my_usb' is created into the root dir

`sudo mount /dev/sdb1 /my_usb/`

`sudo umount /dev/sdb1`

It's very important to unmount a drives if we're working with linux

another command that could be useful is

`sudo blkid`

used to see the UUID of a partition

or we could cat the /etc/fstab to define mountable file systems (pag 295)

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F86594d16-6684-49ea-af6f-50fdab2d5838.png&w=3840&q=80)

## Swap Space

### Windows troubleshooting

The Windows OS uses a program called The Memory manager to handle virtual memory. Its job is to take care of that mapping of virtual to physical memory for our programs and to manage paging. In Windows, pages saved to disk are stored in a special hidden file on the root partition of a volume called page file dot sis. Windows automatically creates page files and it uses the memory manager to copy pages of memory to be read as needed. The operating system does a pretty good job of managing the page file automatically. Even so, windows provides a way to modify the size, number and location of paging files through a control panel applet called System Properties. You can get to the system properties applet by opening up the control panel.

Going to the system and security setting, and clicking on system. Once in the system pane, you can open up the advanced system settings on the left hand menu.

Pick the advanced tab, then click on the settings button in the performance section. One last time, click on the advance tab and you should see a section called virtual memory which displays the paging file size.

If you click the change button, you can override the defaults Windows provides, so you can set the size of the paging file, and add paging files to other drives on the computer.

Microsoft has some guidelines for setting the page in file size that you can follow. For example, on 64 bit Windows 7, the minimum paging file size should be set to 1x, the amount of RAM in the machine. Unless you have a specific reason to change it, it's generally fine to let windows automatically manage the paging file size itself.

### Linux Swap Management #`free -m`

with `free -m` we can see the Ram and Virtual memory usage and conditions

we could create a swap area with the commands we've seen before

`mkpart primary linux-swap [start] [end]`

Anyways, to complete this process, we need to specify that we want to make it swap space with the mkswap command. Let's quit out of parted and run this command on a new swap partition.

`sudo mkswap /dev/sdb2`: this command sets up the space but it's not enabled yet

`sudo swapon /dev/sdb2` : sets on the swap space

`sudo swapoff /dev/sdb2` : sets off the swap space

# Files

## Windows Files

When we talk about data, we're referring to the actual contents of the file; like a text document that we saved to our hard drives. The file metadata includes everything else, like the owner of the file, permissions, size of the file, it's location on the hard drive, and so on. Remember that the NTFS file system is the native file system format of windows. So how exactly does NTFS store and represent the files we're working with on our operating system? NTF uses something called The Master File Table or MFT to keep everything straight.

### MFT

Every file on a volume has at least one entry in the MFT, including the MFT itself. Usually, there's a one-to-one correspondence between files and MFT records. But if a file has a whole lot of attributes, there might be more than one record to represent it. In this context, attributes are things like the name of a file, it's creation time stamp, whether or not a file is read-only, whether or not the file is compressed, the location of the data that the file contains, and many other pieces of information. When you create files on an NTFS file system, entries get added to the MFT.

When files get deleted, their entries in the MFT are marked as Free so they can get reused. One important part of a file's entry in the MFT is an identifier called the file record number. This is the index of the files entry in the MFT.

A special type of file we should mention in Windows is called a shortcut. A shortcut is just another file and another entry in the MFT. But it has a reference to some destination, so that when you open it up, you can get taken to that destination

### PS `mklink`

Besides creating shortcuts as ways to access other files, NTFS provides two other ways using hard and symbolic links. This might get a little weird but stay with me. Symbolic links are kind of like shortcuts but at the file system level. When you create a symbolic link, you create an entry in the MFT that points to the name of another entry or another file. This might seem like just another way to make a shortcut but symbolic links have a key difference. The operating system treats them like substitutes for the file they're linked to in almost every meaningful way.

A symbolic link is connected to the directory of the file (so to his name). if we create an hard link the connection will be created to the MTF entry, so even if we change directory or name file, the link will still work.

`mklink /H filelink.txt file.txt`

### cmd `fsutil repair query C:`

i hope you won't use this command cause I don't want to know what it does. however you know it exists, so you can search for it in your favorite internet browser

Another emergency tool is `chkdsk /F`

## Linux Files

In Linux, metadata and files are organized into a structure called an inode. Inodes are similar to the Windows NTFS MFT records. We store inodes in an inode table and they help us manage the files on our file system.

Shortcuts in Linux are referred to as softlinks, or symlinks. They work in a similar way symbolic links work in Windows, in that they just point to another file.

The other type of link found in Linux are hardlinks. Similar to Windows, hardlinks don't point to a file. In Linux, they link to an inode which is stored in an inode table on the file system. Essentially, when you're creating a hardlink, you're pointing to a physical location on disk or more specifically on the file system.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fb76688cd-94fd-44b1-b7f6-e9d57e8f0189.png&w=3840&q=80)

the red field in this pic is the number of hardlinks a file has. if the pointer reaches the 0, the file is completely removed from the machine.

### Bash `ln -s [filename] [linkname]`

To create a softlink, we can run the command ln with the flag -s for softlink.

If we wanted to create an hardlink we would just remove the -s flag.

`ln [filename] [linkname]`

if we had run the `ls -l [filename]` again, now the counter of hardlinks would be 2.

### Bash `du -h` & `df -h`

This commands gives us the disk usage.

### Bash `sudo fsck [dir]`

To try and repair a file system manually in Linux you can also use the fsck or file system check command.

Just make sure the file system isn't mounted. I won't run this command, but this is what it would look like.

If you run fsck on a mounted partition, there's a high chance that it'll damage the file system. File system repair isn't always a guaranteed fix, but it can help in most cases.

# Process Management

When you open up an application like a word processor, you're launching a process. That processes get in something called a process ID to uniquely identify it from other processes. Our computer sees that the process needs hardware resources to run. So our kernel makes decisions to figure out what resources to give it. Then, in the blink of an eye, our computer starts up a word processor and tadah, already to start working. This happens for every process you launch yourself, and for every process you don't even know who's running.

Besides, the visible processes that we start, like our music player or word processor, there are also not so visible processes running. These are known as background processes, sometimes referred to as daemon processes. Background processes are processes that run in the background. We don't really see them, and we don't interact with them, but our system needs them to function. They include processes like scheduling resources, logging, managing networks, and more.

## **Windows: Process Creation and Termination**

When Windows boots up or starts, the first non-kernel user mode that starts is the Session Manager Subsystem or smss.exe. The smss.exe process is in charge of setting some stuff up for the OS to work. It then kicks off the log-in process called winlogon.exe appropriately enough, along with the Client/Server Runtime Subsystem called csrss.exe, which handles running the Windows GUI and command line council.

In Windows, each new process that's created needs a parent to tell the operating system that a new process needs to be made. The child process inherit some things from its parent like variables and settings, which we can collectively refer to as an environment. This gives the child process a pretty good start in life, but after the initial creation step, the child is pretty much on its own.

Unlike in Linux, Windows processes can operate independently of their parents.

### PS `taskkill /pid [n]`

You can use a command prompt command by calling on the task kill utility. Task kill can find and halt a process in a few ways. One of the more common ways is use an identification number, known as the process id or PID to tell task kill which process you'd like stopped.

One way to do this is to kill notepad again by specifying the PID using taskkill/pid and then the PID number. `Taskkill /pid`, this is the process id of notepad. That's success. This will send the termination signal to the process identified by the PID, which happens to be notepad in our case.

### PS `get-process` & cmd `tasklist`

taskmgr.exe

On the Windows operating system, the task manager or task mgr.exe is one method of obtaining process information. You can open it with the control shift escape key combination or by locating it using the start menu

We find the pid in the detail section in this program or with the two commands above.

## Linux: Process Creation and Termination

In Linux processes have a parent child relationship. This means that every process that you launch comes from another process.

If all processes come from another process, there must be an initial process that started this all, right?

Yes, there is, when you start up your computer, the kernel creates a process called a nit, which has a PID of one. A nit starts up other processes that we need to get our computer up and running. There are more nuances to process creation than this, but I wanted to introduce the parent process concept, since you'll see them when we start managing processes.

### Bash `ps -x`

This shows you a snapshot of the current processes you have running on your system. The ps output can be overwhelming to look at at first, but don't worry, we'll walk through how to read this output.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fed36c4eb-bd1a-4277-8901-8c4a51748320.png&w=3840&q=80)

-   PID: process ID
-   TTY
-   STAT: R - Running; T - Stopped; S - Interruptible Sleep;
-   TIME: total CPU time the process is taking up
-   COMMAND: the name of the command run

### Linux: Analyzing processes in detail

1.  `ps -ef`

the -e lets us see even other users processes

the -f flag is for full details

4.  If we want to search for a process in this messy list we use the grep command

`ps -ef | grep Chrome`

6.  Important: even the processes in Linux are files. so we can see them in a directory:

`ls -l /proc`

every line is a process

9.  To see what the process is doing down to the bone we cat it:

`cat /proc/[PIDnumber]/status`

but this is not very practical. stick to the first command to understand what is going on

## Signals

Imagine you're starting up a video game that's taking a while to render its graphics. You decide that you don't want to play anymore, which leaves you with a few options. You can wait for it to finish loading and then quit the game from the menu, or you can interrupt the process altogether, telling it to quit at the system level.

This is just one example of a time you might find yourself wanting to close a process before it fully completes. To tell a process to quit at the system level, we use something called a signal. A signal is a way to tell a process that something's just happened.

You can generate a signal with special characters on your keyboard and through other processes and software. One of the most common signals you'll come across is called SIGINT, which stands for signal interrupt. You can send this signal to a running process in both Linux and Windows with the CTRL+C key combination.

## Managing Processes

### Windows Process Explorer

Process Explorer is a utility Microsoft created let IT support specialists, systems administrators, and other users look at running processes.

Once you've downloaded Process Explorer and started it up, you'll be presented with a view of the currently active processes in the top window pane. You'll also see a list of the files a selective process is using in the bottom window pane.

This can be super handy if you need to figure out which processes use a certain file, or if you want to get insight into exactly what the process is doing, and how it works. You can search for a process easily in Process Explorer by either pressing Control F, or clicking on the little binocular button.

### Linux `kill`

if you want to be gentle you just `kill [pid]`

if you want to completely bruteforcekill the shit out of the program you're gonna:

`kill -KILL [pid]`

if you want to suspend you use `kill -TSTP [pid]`

to continue `kill -CONT [pid]`

## Resource Monitoring

### Windows Resource Monitoring tool

in Windows, what are the most common ways to quickly take a peek at how the system resources are doing is by using the Resource monitoring tool.

By right clicking and choosing the properties of a process, we go to the performance graph to visually see how much resources the process is taking.

To get the best informations in Poweshell we:

`get-process | sort CPU -descending | Select -first 3 -Property ID,ProcessName,CPU`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F84a52dd1-fc6d-4cd1-8998-dc87af32c098.png&w=3840&q=80)

### Linux Resource Monitoring

### Bash `top`

A useful command to find out what your system utilization looks like in real time is the top command. Top shows us the top processes that are using the most resources on our machine. We also get a quick snapshot of total tasks running or idle, CPU usage, memory usage, and more.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F9ba2d334-3a9c-4498-9819-3f3ad3ef7986.png&w=3840&q=80)

### Bash `lsof`

Another command that you can use to help manage processes is the lsof command. Let's say you have a USB drive connected to your machine, you're working with some of the files on the machine, then when you go to eject the USB drive, you get an error saying, device or resource busy. You've already checked that none of the files on the USB driver is in use or opened anywhere, or so you think. Using the lsof command, you don't have to wonder. It lists open files and what processes are using them**.**

This command is great for tracking down those pesky processes that are holding open files.

# Useful & Practical

## Remote Connections and SSH

SSH or secure shell is a protocol implemented by other programs to securely access one computer from another. To use SSH, you need to have an SSH client installed on the computer you're connecting from along with an SSH server on the computer you're trying to connect to. Keep in mind that when we say SSH server, we don't mean another physical machine that serves a data. An SSH server is just software.

The most popular program to use SSH within Linux is the OpenSSH program. We'll talk about how to use SSH from a Windows machine using the popular Open Source program PuTTY.

![PuTTY is a free open-source software that you can use to make remote connections through several network protocols including SSH](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F1f980542-4b1f-4ff8-a47f-b09426ada5b6.png&w=3840&q=80)

PuTTY is a free open-source software that you can use to make remote connections through several network protocols including SSH

### Windows RDP client

Microsoft actually provides another way to connect to other Windows computers called the Remote Desktop Protocol or RDP. There are also RDP clients for Linux and OS 10 too, like RealVNC and Microsoft RDP on Mac. We'll add some links to these clients in the supplemental reading. RDP provides users with a graphical user interface to remote computers, provided the remote computer has enabled incoming RDP connections.

A client program called the Microsoft Terminal Services Client or mstsc.exe is used to create RDP connections to remote computers. You can enable remote connections on your computer by opening up the start menu, right clicking on "This PC", then selecting "Properties". Then Remote settings.

You can launch the RDP client in a few ways. You can type "mstsc" at the run box or look up Remote Desktop connections in the Start Menu.

Once you've launched the client, it'll ask for the name or IP address of the computer you want to connect to. The Windows RDP client can also be launched from the command line, where you can specify more parameters like slash admin if you want to connect to the remote machine with administrative credentials.

[https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

## File Transfer

### Linux SCP and SSH File Transfer

SCP, or secure copy, is a command you can use in Linux to copy files between computers on a network.

It utilizes SSH to transfer the data. So just like you would SSH into a machine you can send a file that way.

Let's see this in action. Let's say you want to copy over a file from our computer to another computer. To do this, we can run the SCP command with a few flags.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F79bafddf-1263-47e2-9235-263e8517917b.png&w=3840&q=80)

### Windows Shared Folders

Shared folders do pretty much what you'd expect from their name. You tell Windows you want to share a folder with a person or group of people, then drop some files into it. Anyone you've shared the folder with can then access those files. Sharing folders in Windows is easy. Just right-click on the folder you want to share, Then mouse over the Share with option, And then pick specific people from here.

Once you've shared the folder, you can access it from other computers. Start by opening up This PC, Then going into the Computer tab. And from here, you can map the folder directly to your computer with the map network drive option.

Finally, on another computer, you can visit it directly from the run box by typing in backslash, whatever the computer name is, and then backslash the folder name that you mapped it to. You might be interested to know that you can share folders from the command line too, using the net share command. Net share lets you do the same thing as the GUI sharing workflow, and you'll need to specify what kind of permissions you'd like to give which users. Let's say you wanted to give everyone on your network full permissions to a folder called shareme.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F322a6f5b-5129-41c0-aca3-f41311cb8704.png&w=3840&q=80)

# Logs

## System Monitoring

### Windows Event Viewer

In Windows, the events logged by the operating system are stored in an application called the Event Viewer. Whether you're trying to figure out why a computer game keeps crashing, or troubleshooting login or access problems, or just satisfying your curiosity about what's going on in your system, the Event Viewer is a great first stop.

You can launch the Event Viewer either from the start menu or by typing in eventvwr.msc from the run box.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fd685625d-3c36-4b84-ba7c-64e9fdc98d86.png&w=3840&q=80)

The first group we see is called Custom Views. The Event Viewer records a lot of information about the system. So it can sometimes be a little difficult to tease out the signal, like recent events, from the noise or the stuff you don't care about.

> custom view: last hours, only windows logs.

**other tips**

Let's say you're having an issue with a driver failing during startup. The log called System would be a good place to start. If you want to see whose been accessing the computer, then you begin investigating the Security log.

The other category is called Applications and Services Logs. This category contains logs that track events from a single application, or operating system component, instead of the system wide events of the Windows logs category. For example, if you're having trouble with PowerShell and wanted to get more information about it, checking out the PowerShell log under Applications and Services log would be a great first step.

### Linux Logs

Logs in Linux are stored in the /var/log directory. Remember that /var directory stands for variable, meaning, files that constantly change are kept in this directory, and it turns out that logs are constantly changing.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F385c2563-4715-49e2-b2a3-d0b8ae11b0f9.png&w=3840&q=80)

Let's check out some of the common ones you'll look at, /var/log/auth.log, authorization and security-related events are logged here. /var/log/kern.log, kernel messages are logged here. /var/log/dmesg, system startup messages are logged here. If you encounter an issue at, let's say, boot up, this is a good place to check for information.

It might get a little tiresome to open up each of these log files to find information about events. Luckily, there are also log files that combine the information of other log files.

The one log file that logs pretty much everything on your system is a /var/log/syslog file. The only thing that sys log doesn't log by default are off events. When troubleshooting issues with user machines, **/var/log/syslog** will usually contain the most comprehensive information about your system, so that should be your first stop.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F559d2a66-eaba-493f-98cb-7230917bc157.png&w=3840&q=80)

The first field here is the time stamp when the event occurred. Pretty straightforward. But depending on the log, you might see a time format you aren't familiar with like a long string of numbers such as 1501538594.

The next field is the host name of the machine the event occurred on. Next up is the service that the log event is referring to. And last is the event that occurred.

Time stamps found in a format like this are referred to as Unix or epoch time. At first, you might be baffled by this. Why would you represent time in this way? And just what exactly is the Unix epoch? The Unix epoch time is used to represent, then, it's the number of seconds since midnight on January first, 1970, a sort of zero hour for Unix based computers to anchor their concept of time. This means that 1501538594 represents the date, time, Monday, July 31st. 30314 Pacific Standard Time. Why midnight on January first, 1970? Is that date the birthday of Unix? Or does it mark some other significant event? The actual answer is much simpler. The original engineers of Unix at Bell Labs just picked it because it was convenient.

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2Fbf789c2d-327b-4520-9115-21c31dcce157.png&w=3840&q=80)

Let's say every time you launch a specific application, it does something abrupt and shuts off. Sure, you can check the logs and post and keep track of your time or you can look at the logs in real time. To do this, we can use one of the commands we learned in a very early lesson, tail. Let's take a look at what this means. We're going to tail -f the syst log file and keep it in an open window.

# System Deployment

## Bash `dd`

![image](https://matteocapo.super.site/_next/image?url=https%3A%2F%2Fsuper-static-assets.s3.amazonaws.com%2F02c48622-a5fb-42b8-97f4-5ccc534fdb55%2Fimages%2F628da336-ff54-4ce9-a454-1dbfcbbbc6e1.png&w=3840&q=80)

You don't have to know how dd works to use this command. Actually, you should check out the final supplemental reading to learn more about this tool. This just says, I'm going to copy the contents of /dev/SDD which is the USB drive and save it to the desktop in an image file. Once the image file is saved, if we open it up we should see the exact same contents as the USB drive.