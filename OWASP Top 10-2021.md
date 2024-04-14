The [*Open Web Application Security Project (OWASP)*](https://owasp.org) is a non-profit-organistaion dedicated to enhance the security of web services and applications

# 1. Broken Access Control
Websites have pages not meant to be accessed by regular users. If the authorisatoin can be bypassed and these pages can be accessed the *access control* of the page is broken which can lead to:
- view sensitive information from other users
- gaining unauthorized functionality

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

# 3. Injection
