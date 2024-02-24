# Running Your First Commands

A big selling point of Linux is that they can be lightweigth. Often there is no *Graphical User Interface (GUI)* to interact with, instead you interact with the *Terminal*, which is text-based. 

The first commands to start with are:

| Command | Description |
| :--: | :--: |
| echo | Output any text provided |
| whoami | Display the current user |
```
tryhackme@linux1:~$ echo "Hello World!"
tryhackme@linux1:~$ whoami
```
# Interacting With the Filesystem!

The following commands will help you to navigate the filesystem:

| Command | Full Name | Purpose |
| ---- | ---- | ---- |
| ls | listing |  |
| cd | change directory |  |
| cat | concatenate |  |
| pwd | print working directory |  |
| touch | touch | create a file |
| mkdir | make directory | create folder |
| cp | copy | copy a file or folder |
| mv | move | move a file or folder |
| rm  | remove | remove a file or folder |
| file | file | display the type of a file |
## Listing Files in the Current Directory (ls)

The ls command lists the files and folder contained in the current directory:
```
tryhackme@linnux1:~$ ls
> 'Important Files'  'My Documents'  Notes Pictures
```
You can lists the content of a directory without navigating to it: `ls Pictures`

## Changing the Current Directory (cd)

You can change the current directory with the `cd` command:
```
tryhackme@linux1:~$ cd Picutres
tryhackme@linux1:~/Pictures$ 
```
You can change to directory by entering the path of it: 
``` cd /home/Ubuntu/Documents```

## Outputting the Contents of a File (cat)

With the `cat` command you can output the content of text files.
```
tryhackme@linux1:~/Documents$ ls
> todo.txt
tryhackme@linux1:~/Documents$ cat todo.txt
> Here is something important for me to do later!
```

You can output the content of a file without noavigating to it:
`cat /home/ubuntu/Documents/todo.txt`

## Display the Path of the Current Working Directory

Use the `pwd` ("print working directory").
```
tryhackme@linux1:~/Documents$ pwd
> /home/ubuntu/Documents
tryhackme@linux1:~/Documents$
```

## Creating Files and Folders (touch, mkdir)
Create files and folders by using `touch fileName` or `mkdir folderName`
```
tryhackme@linux2~$ touch note
tryhackme@linux2:~$ mkdir mydirectory
tryhackme@linux2:~$ ls
> folder1 mydirectory note
```

## Removing Files and Folders (rm)
To remove files use `rm filename`, for directories `rm -R directoryName`
```
tryhackme@linux2:~$ rm note
tryhackme@linux2:~$ rm -R mydirectory
tryhackme@linux2:~$ ls
> folder1
```
## Copying and Moving Files and Folders (cp, mv)
Use `cp sourceFile destinationFile` to copy a note into anotherone. 
Moving a file with `mv sourceFile destinationFile` will not copy or create a new file ,but merge the first with the second file. `mv` can also be used to rename files.
```
tryhackme@linux2:~$ cp note note2
tryhackme@linux2:~$ ls
> folder1 note note2

tryhackme@linux2:~$ mv note2 note3 
tryhackme@linux2:~$ ls 
> folder1 note note3
```

## Determining File Type
Use `file fileName` to determine the type of the file
```
tryhackme@linux2:~$ file note 
> note: ASCII text
```
# Searching for Files

## Using Find
`find` can be used to search for files either by entering the name or the data type of the file with a wildcard `*` , which will display every file with that ending:  
```
tryhackme@linux1:~$ find -name password.txt
> ./folder1/password.txt

tryhackme@linux1~$ find -name *.txt
> ./folder1/password.txt
> ./Documents/todo.txt
```
## Using Grep

`grep` lets you search the contents of files for specfic values or words.
```
tryhackme@linux1:~$ grep "81.143.211.90"
> access.log
> 81.143.211.90 - - [25/Mar/2021:11:17 + 0000]
> "GET / HTTP/1.1" 200 417 "-" "Mozilla/5.0 (Linux; Android 7.0; Moto G(4))"
```
# Introduction to Shell Operators
| Symbol | Description |
| :--: | :--: |
| & | Allows commands to run in the background |
| && | Allows you to combine multiple commands |
| > | Redirects the output from a command somewhere else |
| >> | Appends the output of a command somewhere |
## Operator "&"
Allows you to run processes in the background like downloading or copying a large file.

## Operator "&&"
Combines multiple commands, for example: `command1 && command2`

## Operator ">"

Known as the output redirector it redirects the output of a command somewhere else.
A example would be to redirect the output of a `echo` command to a file.
```
tryhackme@linux1:~$ echo hey > welcome
tryhackme@linux1:~$ cat welcome
> hey
```

## Operator ">>"
This operator will unlike the previous operator not replace but append the output of a command to a file.
```
tryhackme@linux1:~$ echo hello >> welcome
tryhackme@linux1:~$ cat welcome
> hey
  hello
```
# Accessing Linux using SSH

## What is SSH & how Does it Work

**Secure Shell (SSH)** is a protocol that allows you to send commands on a remote device in an encrypted channel.
## Using SSH

To use SSH you need:
1. the IP of the system
2. valid login credentials

The syntax for SSH is `ssh username@IP-address` followed by the password
```
root@ip-10-10-81-203:~# ssh tryhackme@10.10.224.147
> password:
tryhackme
> tryhackme@linux2~$
```

# Introduction to Flags and Switches
Commands will, when not specified, perform their default behavior. For example `ls` will display the the content of a directory but not hidden files. To do that we can use the flag `-a`  (short for `--all`).
```
tryhackme@linux2:~$ ls 
> folder1
tryhackme@linux2:~$ ls -a 
> .hiddenfolder folder1
```
For a list of the different flags and switches use ` command --help`.

## The Manual Page
To access the documentation of a command use `man command`.

# Permissions 101
Not all users can access all files and folders.

## Switching Between Users
Use the `su` command to switch between users on a machine. By entering `su -l` we inherit properties like enviroment variables of the new users and are also dropped into the home directory of the user.
```
tryhackme@linux2:~$ su user2 
Password:
user2@linux2:/home/tryhackme$

tryhackme@linux2:~$ su -l user2 
Password:
user2@linux2:~$ pwd 
user2@:/home/user2$
```

# Common Directories
## /etc
This is one of the most important root directories, used to store system files used by the OS.

```
tryhackme@linux2:/etc$ ls 
> shadow passwd sudoers sudoers.d
```
The sudoers file contains a list of users and groups that are allowed to run `sudo` or commands as the root user. "passwd" and "shadow" show how the Linux OS stores passwords for each user in encrypted sha512 format.

## /var
This directory stores data that is frequently accessed or written by services or applications; for example log files, written in /var/log, and data not associated with a specific user, like databases.
```
tryhackme@linux2:/var$ ls 
> backups log opt tmp
```

## /root
This is the home folder of the "root" user.

## /tmp
This root directory is used to store data that is used once or twice. Interesting for pentesting is that any user can write into it, once we have access to the system we can store data like enumeration scripts here.
```
root@linux2:/tmp# ls 
> todelete trash.txt rubbish.bin
```

# Terminal Text Editors
## Nano

To edit a file use `nano fileName` and the editor will launch. Nano covers the most needs of what you want from an editor:
- Searching for text
- Copying and Pasting
- Jumping to a line
- Finding out what line your are on

To use the features press "crtl", represented as "^" on Linux, and the corresponding letter.
## VIM
VIM is a more advanced text editor, with features like:
- Customisable: modify the keyboard shortcuts
- Syntax ighlithing
- VM works on all terminals
- Lots of resources like [cheatsheets](https://vim.rtorr.com/) 

# General/Useful Utilities
## Download Files (wget)

The `wget` command allows you to download files from the web, by providing the address of the resource we want to download.
```
wget https://assets.tryhackme.com/additional/linux-fundamentals/part3/myfile.txt
```

## Transfering Files From Your Host (scp)

This command allows you to securely copy files from and to a remote system using the SSH protocol to provide authentication and encryption. 
The syntax is `scp source destination.`
`scp` has some variables that you need to provide, to copy, for example a local file to a remote system:

|                               Variable                                |     Value      |
| :-------------------------------------------------------------------: | :------------: |
|                    IP address of the remote system                    |  192.168.1.30  |
|                       User on the remote system                       |     ubuntu     |
|                           Name of the file                            | important.txt  |
| Name of the file on the remote system <br>in which the copy is stored | transfered.txt |
The command to copy the local file on the remote system would be:
```
scp important.txt ubuntu@192.168.1.30:/home/ubuntu/transferred.txt
```

To copy a remote file to our machine we would use:
```
scp ubuntu@192.168.1.30:/home/ubuntu/documents.txt notes.txt
```

## Serving Files From Your Host (web)
Python3 offers the module "HTTPServer", pre-packaged on Ubuntu machines, which allows your computer to act as a web server where you can offer your own files for downloading with commands such as `curl` and `wget`.
By default  Pyton3's "HTTPServer" will serve the filles in the directory where you execute the command, the options for other behavior can be found in the `man` pages.

To start the server use `python3 -m http.server`

```
tryhackme@linux3:/webserver# python3 -m http.server 
Serving HTTP on 0.0.0.0 port 8000 
(http://0.0.0.0:8000/) ...
```

To download files use the `wget` command and enter the name of the file and the IP address of the computer:
```
tryhackme@mymachine:~# wget http://MACHINE_IP:8000/myfile
```

A flaw of this module is that you have to know the file name, there is no option to index the content of the directory. An alternative but more advanced webserver is [Updog](https://github.com/sc0tfree/updog).

# Processes 101

Processes are the programs running on a compute and are managed by the kernel. A *Process ID (PID)* is assigned to each one, which will increment for each new process (the 10th process has the PID of 10).

## Viewing Processes
The `ps` command lists all active processes of the user session along with additional information like, status code, the session running it and the usage time of the CPU it is using.

To view the processes of other users and system processes you use the `ps aux`.

The `top` command gives you real time statistics of the processes that are tunning and are refreshed every 10 seconds or when you use the arrow keys to browse the rows.

## Managing Processes

There are many types of signals we can use to terminate processes, with the difference being how "cleanly" theses are terminated. The commnad `kill PID` will kill a process.
Some other signals are:
- *SIGTERM*:  allow the process to cleanup tasks before terminating  
- *SIGKILL*: termminate the process without cleanup
- *SIGSTOP:* Stop/supsend the process

## How do Processes Start?
