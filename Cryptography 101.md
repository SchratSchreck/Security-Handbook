
# Terms

| Term                    | Discription                                                                                     |
| ----------------------- | ----------------------------------------------------------------------------------------------- |
| *Alice and Bob*         | Representation of 2 people communicating                                                        |
| *Asymmetric encryption* | Uses different keys to encrypt and decrypt                                                      |
| *Brute Force*           | Attack trying to crack cryptography by trying multiple different inputs, f.e. passwords or keys |
| *Cipher*                | A method of encrypting or decrypting data                                                       |
| *Ciphertext*            | Encrypted data                                                                                  |
| *Cryptanalysis*         | Attacking cryptography by finding weaknesses in the underlying math                             |
| *Encoding*              | form of representing data, f.e. Base64 or hexadecimal (this is *not* a form of encryption)      |
| *Encryption*            | Transforming data (plaintext) into ciphertext with the use of a cipher                          |
| *Key*                   | Data needed to de- (and/or) encrypt the the ciphertext into plaintext                           |
| *Symmetric encryption*  | Uses different keys for de- and encryption                                                      |

- **Cipher:**
- **Plaintext:** Data that is not encrypted or hashed 
- **Encoding:** form of representing data, f.e. Base64 or hexadecimal (this is *not* a form of encryption)
- **Hash:** output of a hash function 
- **Brute Force:** Attack trying to crack cryptography by trying multiple different inputs, f.e. passwords or keys
- **Cryptanalysis:** Attacking cryptography by finding weaknesses in the underlying math
# Hashing
**Hash functions**
A hash function takes an input of data and creates a "digest" or summary of it with a fixed length. The output will be easy to compute but hard to reverse back to the input and to be predicted. Even changing only a single bit will cause a large change in the output. 
The output is normally in raw bytes and then encoded (f.e. base64 or hex), but decoding it will still give no information regarding the input.

**Hash collisions**
A hash collision occurs when 2 different inputs result in the same output. While hash function will try to avoid these collisions as best as they can, they are not unavoidable

**Uses for Hashing**
Hashes are mostly used to verify the integrity of data or verifying passwords.
 
 **Verifying Passwords**
 A hash of a password is stored in you database and are compared to the user input when they attempt to login.
 A problem that could occure is when two user have the same password, resulting in the same hash and when an attacker can crack the hash they can create a lookup table, listing the hash for different passwords. To avoid this a unique "salt" is added to the passwords and stored in the database. This salt is either added at the start or end of the password before being hashed, ensuring that every user will have a unique hash. 

**Recognising Password Hashes**
There are automated hash recognition tools, like [hashID](https://pypi.org/project/hashID/) or [Hash Verifier](https://hashes.com/en/tools/verify), but these can be unreliable for hashes without a prefix.
- *Unix* style password hashes have a prefix, telling us the algorithm used to compute the hash. The format for the hash is: `$format$rounds$salt$hash`
- *Windows* password hashes use NTLM, a variant of md4 and visually identical; it is important to use context to identify the hash

Password hashes are stored:
- On *Windows* in the *Security Accounts Manager (SAM)* and are split into NT and LM hashes; tools like [mimikatz](https://github.com/ParrotSec/mimikatz) can be used to dump the password hashes
- On *Linux* they are stored in /etc/shadow, which is only readable by root; they used to be stored in /etc/passed and readable by everyone

**Most Common Unix Style Prefixes**

| Prefix                        | Algorithm   | Example                             |
| ----------------------------- | ----------- | ----------------------------------- |
| `$1$`                         | md5crypt    | Cisco, older Linux/Unix systems     |
| `$2$, $2a$, $2b$, $2x$, $2y$` | Bcrypt      | web application                     |
| `$6$`                         | sha512crypt | default for most Linux/Unix systems |
A list with more hash formats can be found [here](https://hashcat.net/wiki/doku.php?id=example_hashes).
## Password Cracking
Tools like [Hashcat](https://hashcat.net/hashcat/) and [[John the Ripper]] "crack" passwords by hashing a large set of passwords ( often fram wordlists like rockyou), adding salt if there is one and comparing it to the target hash.
There are also online tools like [hashes.com](https://hashes.com/en/decrypt/hash) to crack hashes.

**Cracking on Graphic Processing Unit (GPU)**
	Graphic cards have thousands of cores, which are very good at maths (including hash functions) and can  crack hashes quickly. Some hash algorithm, f.e. bcrypt are designed so that hashing on a GPU is the same speed as hashing on a CPU, macking cracking them more difficult.

**Integrity Checks with Hashing**
Hashing is often used to ensure that a file has not been changed, as the same input will result in the same output, meaning if a single bit is changed the result will be different.
The *HMACs* method is used to verify the integrity and authentication of data by using a secret key (ensures authentication) and a hashing algorithm (ensures integrity) to compute a hash. 

# Encryption
Cryptography is used to ensure confidentiality, integrity and authenticity, by providing encrypted connections, certificates and the possibility to checksum (downloaded) data.
Encryption is also important when you handle data standards like [PCI-DSS](https://listings.pcisecuritystandards.org/documents/PCI_DSS_for_Large_Organizations_v1.pdf) state that data should be encrypted both at rest (in storage) and while being transmitted.
## Types of Encryption

**Symmetric Encryption**
Uses the same key to encrypt and decrypt data. These algoithms are faster and use smaller keys (128 or 256 bit keys)

**Asymmetric Encryption**
Uses a pair of keys, one to encrypt data (private key) and the other to decrypt the data (public key). These algorithms are slower and have larger keys (f.e. RSA with 2048 to 4096 bit keys). 
## Rivest Shamir Adleman (RSA) 
**The Math of RSA**
RSA is based on the mathematical problem of finding the factors of a large number. Multiplying two prime numbers is easy (f.e. 17 x 23 = 391), but its difficult to find which prime numbers need to be multiplied to make 14351.
The key variables in RSA are:

| Variable | Function                  |
| -------- | ------------------------- |
| *p*      | first large prime number  |
| *q*      | second large prime number |
| *n*      | product of *p* and *q*    |
| *n*      | public key                |
| *d*      | private key               |
| *m*      | plaintext                 |
| *c*      | ciphertext                |
For more on the math behind RSA you can read https://muirlandoracle.co.uk/2020/01/29/rsa-encryption/.

**Attacking RSA**
RSA is often found in *CTFs*, where you need to calculate variables or break the encryption.
The Wikipedia page for [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) is a good source to find information.
Tools which can come in handy are:
- https://github.com/Ganapati/RsaCtfTool
- https://github.com/ius/rsatool

# Establishing Keys using Asymmetric Encryption

# Digital Signatures and Certificates
# Diffie Hellman Key Exchange

# PGP, GPG and AES
**Pretty Good Privacy (PGP)**
This is a software that implements encryption for files, digital signatures and more

**GPG**
Is an Open Source Implementation of PGP, which you may need to use to decrypt files in CTFs. With PGP/GPG private keys can be protected with passphrases, you can attempt to crack these with [[John the Ripper]] and gpg2john. 

**Advanced Encryption Standard (AES)**
AES has replaced DES.