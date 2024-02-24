# Introduction
The **Operating System (OS)** allows the software of a computer system to interact with the hardware.

The **hardware** includes the screen, keyboard, printer USB flash, memory and motherboard. The **motherboard** contains the central processing unit (*CPU*) and memory *(RAM*), the board is usually connected to a *storage device* (HDD or SSD). All other hardware is connected to the *motherboard*, but without an OS we can not run programs or applications on them. The OS is between the hard- and software.
OS are sometimes designed to run on Laptops, some are specifically designed for smartphones (Android, iOS) or servers (IBM AIX, Oracle Solaris, Windows Server).

On your system will most likely be private data like:
- Private conversations
- private photos
- Email client
- Passwords
To ensure that no one you do not trust has access to those data, you must secure your device and OS.

When talking of data security there are three important aspects:

- ***Confidentiality:*** files and information should only be available to people you want to
    
- ***Integrity:*** files and information can not be tempered with
    
- ***Availability:*** your device should be available to you anytime you decide to use it.
## Common Examples of OS Security
### Authentication and Weak Passwords
Authentication is the act of verifying your identity, either local or remote system. This can be achieved through:

- Something you know (password, PIN, …)
    
- Something you are (fingerprint)
    
- Something you have (phone number, …)
    

Passwords are the most common form of authentication and many users tend to use weak passwords or the same password for many websites. Some users use personal details like date of birth, name of their pets and so on.

The *National Cyber Security Centre (NCSC)* published a list of the most common passwords. The top ten are:

| **Rank** | **Password** |
| :--: | :--: |
| 1 | 123456 |
| 2 | 123456789 |
| 3 | qwerty |
| 4 | password |
| 5 | 111111 |
| 6 | 12345678 |
| 7 | abc123 |
| 8 | 1234567 |
| 9 | password1 |
| 10 | 12345 |
Often the letters for passwords are often next to each other and predictable.
### Weak File Permissions

Proper security dictates the *principle of least privilege*. Files and data should only be accessible to people who need it. For example they need it to get their work done. Weak file permission makes it easy to attack confidentiality, accessing files they should not be able to, and integrity, changing files and data.
### Access to Malicious Programs

Programs such as **Trojan horse** can give attackers *access* to your system. **Ransomware** attacks availability, by *encrypting* the user’s data and decrypting them after the ransom is payed.

## Practical Example of OS Security

In this example we gain access to a remote Linux system, by obtaining a username and a password. Next we escalate our privileges to a system administrator, called “root” on Android, Apple and Linux and “administrator” on MS Windows. These accounts have unrestricted access to a system.

We were hired to check the security of a company. When visiting the office we saw a sticky note with “sammie” and “dragon” written on it.

First we open a terminal and use the command  `ssh sammie@10.10.193.96`. With this we connect to the machine 10.10.193.96 and check if “dragon” is Sammie’s password. After entering "dragon" we successfully connected to the machine and have access to it. 
To confirm this we enter the command `whoami`, which should return "sammie" in the terminal.
To list the files in the current directory we can use the command “`ls`”:
```
sammie@beginner-os-security:~$ ls
country.txt  draft.md  icon.png  password.txt  profile.jpg
```  

To display the content of a text file use “`cat` filename”:
```
sammie@beginner-os-security:~$ cat password.txt
sammie
dragon
```

The last command to cover is “`history`” which will display the commands used by a user.

Two other usernames we have are “linda” and “johnny” and we know that both have little regard to cybersecurity.

Using the command “`su – name`”, I changed to johnny’s account with the password “abc123”.
```
sammie@beginner-os-security:~$ su - johnny
Password:
johnny@beginner-os-security:~$
```
If not logged in with a user account I should have used “`ssh johnny@10.10.193.96`”. Using the same command I changed to Linda’s account with the password “qwertyuiop”.