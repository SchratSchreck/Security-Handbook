
# Web Application Security 

## Introduction

We will introduce Web Application Security with by an Online Shop.

**Web applications** are programs running on a server. [[How the Web works#How Web Servers work |Servers ]] are machines running continuously to *serve* the clients. In the case of an online shopping application the app will read the details from the database, used to store and organize information.

The database server is responsible for `reading`, `searching` and `writing` to the database.

The shop might need multiple servers for:
- Product database containing details about the products
- Customer database containing details about the costumers
- Sales database containing the purchases of each costumers

#### The steps for searching an item in the shop are:
1. The user enters an item name or related keywords in the search field. The web browser sends the search keyword(s) to the online shopping web application.
2. The web application queries (searches) the products database for the submitted keywords.
3. The product database returns the search results matching the provided keywords to the web application.
4. The web application formats the results as a friendly web page and returns them to the user.
### Security Risks

Companies offer bug bounty programs, which offer rewards for finding vulnerabilities.

 If you want to buy an item in an online shop, the steps might be:
1. Log in the website
2. Search for the product
3. Add the product to the shopping cart
4. Specify the shipping address
5. Provide payment details

 There are related attacks to some of the steps above:
- Log in the website: The attacker can try to discover the password by using a long list of passwords and an automated tool to test them
- Search for the product: The attacker can try to breach the system by adding specific characters and codes into the search term, with the objective that the system returns data or executes a program it should not
- Provide payment details: The attacker could check if the payment details are sent in plain text or with weak encryption.

#### Identification and Authentication Failure
- `Identification` the ability to identify a user uniquely. 
- `Authentication` is the ability to prove that the user is who they claim to be.

A system needs to confirm a user’s identity and authenticate them before they can access the system.

 Weaknesses for this step are:
- **Allowing brute force** try a lot of passwords, usually using automated tools, to find valid credentials
- **Allowing users to have weak passwords** these are easy to guess
- **Storing user information in plain text** If the attacker gets access to the file they can learn the passwords 

#### Broken Access Control
Access control that a user can only access files (documents, images, networks, …) related to their role or work. Someone from the marketing department should not be able to access the finance department’s documents.
Weaknesses for this category are:
    • Failing to apply the principal of least privilege: giving users more access permission than needed
    • Modifying someone else’s account by using it’s unique identifier: A bank client should not be able to view the transaction of another client.
    • Browsing pages requiring authentication (logging in) as an unauthenticated user: You cannot view web mail before logging in.
    
#### Injection
Injection refers to inserting (malicious) code as part of ones input for example in the search bar or login.

#### Cryptographic Failures
Cryptography focuses on the encryption and decryption of data, ensuring that without the right key you can not understand the data.
Weaknesses are:
    • Sending data over HTTP instead of HTTPS: HTTP sends data without encryption, whereas HTTPS does
    • Relying on weak cryptographic algorithms
    • Using default or weak keys
    
#### Practical Example 
This example will investigate a website with Insecure Direct Object Reference (IDOR) which falls under Broken access control. A user might have access to a file named “IMG_1003.JPG” on a server, there could also be “IMG_1002.JPG” and “IMG_1004.JPG” but the server should not allow access to these even if the user figured out the name of them. In this case IDOR places to much trust on the input data of a user without validating if the user has the permission for it. 
Even when providing a correct URL, it should not automatically give access to the page. The URL “https://store.tryhackme.thm/customers/user?id=16” returns the user with id=16, it should not be possible to access other users by changing the id to other numbers like id=15 or id=17. 
The inventory management system of your company was hacked and the orders for tires where changed. It is your task to find the hacked account and revert the orders.


Your account has the id 11 after changing the id in the url a couple of times I found the hacked account with the id 9 and reverted the orders by clicking the buttons.

Operating System Security
Introduction
The Operating System (OS) allows the software of a computer system to interact with the hardware. 
The hardware includes the screen, keyboard, printer  USB flash, memory and motherboard. The motherboard contains the central processing unit (CPU) and memory (RAM), the board is usually connected to a storage device (HDD or SSD). All other hardware is connected to the motherboard, but without an OS we can not run programs or applications on them. The OS is between the hard- and software.

OS are sometimes designed to run on Laptops, some are specifically designed for smartphones (Android, iOS) or servers (IBM AIX, Oracle Solaris, Windows Server). 
On your system will most likely be private data like:
    • Private conversations
    • private photos 
    • Email client
    • Passwords
To ensure that no one you do not trust has access to those data, you must secure your device and OS.
When talking of data security there are three important aspects:
    • Confidentiality: files and information should only be available to people you want to
    • Integrity: files and information can not be tempered with
    • Availability: your device should be available to you anytime you decide to use it.
 Common Examples of OS Security
Authentication and Weak Passwords
Authentication is the act of verifying your identity, either local or remote system. This can be achieved through:
    • Something you know (password, PIN, …)
    • Something you are (fingerprint)
    • Something you have (phone number, …)
Passwords are the most common form of authentication and many users tend to use weak passwords or the same password for many websites. Some users use personal details like date of birth, name of their pets and so on.
The National Cyber Security Centre (NCSC) published a list of the most common passwords. The top ten are:

Often the letters for passwords are often next to each other and predictable. 
Weak File Permissions
Proper security dictates the principle of least privilege. Files and data should only be accessible to people need it. For example they need it to get their work done. Weak file permission makes it easy to attack confidentiality, accessing files they should not be able to, and integrity, changing files and data.  

Access to Malicious Programs
Programs such as Trojan horse can give attackers access to your system. Ransomware attacks availability, by encrypting the user’s data and decrypting them after the ransom is payed. 
Practical Example of OS Security
In this example we gain access to a remote Linux system, by obtaining a username and a password. Next we escalate our privileges to a system administrator,  called “root” on Android, Apple and Linux and “administrator” on MS Windows. These accounts have unrestricted access to a system.
We were hired to check the security of a company. When visiting the office we saw a sticky note with “sammie” and “dragon” written on it. 
First we open a terminal and use the command “ssh sammie@10.10.193.96”. With this we connect to the machine 10.10.193.96 and check if “dragon” is Sammie’s password.
After entering “dragon” we successfully connected to the machine and now have access to it.
To confirm this we enter the command “whoami”.
To list the files in the current directory we can use the command “ls”:

To display the content of a text file use “cat filename”:

The last command to cover is “history” which will display the commands used by a user 

Two other usernames we have are “linda” and “johnny” and we know that both have little regard to cybersecurity.
Using the command “su – name”, I changed to johnny’s account with the password “abc123”.

If not logged in with a user account I should have used “ssh johnny@10.10.193.96”.  Using the same command I changed to Linda’s account with the password “qwertyuiop”.
