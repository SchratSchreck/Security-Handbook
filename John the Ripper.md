John the Ripper is a usefull hash cracking tool, which uses dictonary attacks to crack passwords. A dicronary attack hashes a large number of words using the targets hash algorithm and compares the output with the hash you try to crack.

# Setup
You can install John the Ripper using `sudo apt install john` when using Kali Linux or check if it is install by typing `john` into the terminal.

**Install from Source for Linux**
1. To clone the jumbo john repository use:
```
git clone https://github.com/openwall/john -b bleeding-jumbo johns
```  
1. Change your current directory `cd john/src/` 
2. Check the required dependencies and options that have been configured with `./configure`
3. To build a binary of john use `make -s && make -sj4`, which is stored in the directory above; you can change to it with `cd ../run`
4. Test the binary with `./john --test`

For more info look [here](https://github.com/openwall/john/blob/bleeding-jumbo/doc/INSTALL).

**Installing on Windows**
Just install the zipped binary of Jumbo John for either 64 bit or 32 bit systems.
# Wordlist
As mentioned you need a large amount of words to carry out an effective dictonary attack. The files in which these words are stored/found are called wordlists and a good collection of these can be found in the [SecLists](https://github.com/danielmiessler/SecLists) repository.
On Parrot and Kali Linux you can find a collection of wordlists in the `/usr/share/wordlists` directory.
# Basics
**Basic Syntax**
The basic syntax for John is `john [options] [path-to-hash]`

**Automatic Cracking**
John has the option to automaticaly detect the type of hash you want to crack, but this can be unrelyable.
```
john --wordlist=[path to wordlist] [path to hash]:
john --wordlist= /usr/share/wordlists/rockyou.txt hash_to_crack.txt
```

**Identifying Hashes**
To identify hashes, in the case John fails, you can use [online tools](https://hashes.com/en/tools/hash_identifier) or use tools like [hash-identifier](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master) which you can install using:
```
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
``` 
To start it use `python3 hash-id.py`.

**Format-Specific Cracking**
When you know the type of hash you need to crack you can tell john to use it
`john --format=[format] --wordlists= [path to wordlist] [path to hash]`
Note:
When you need to use standard hash types, you need to prefix them with `raw-`. To see what prefix you need use `john --list=formats` and check manually or use, f.e. `john --list=formats | grep -iF "md5"`

**Exercise**
# Windows Authentication Hashes
Authentication Hashes are hashed passwords stored by operating systems. You usually need a privileged user account to get these by dumping the SAM database with tools like Mimikatz. It is not always necessary to crack the hash after obtaining it, as you can perform a "pass the hash attack".

**NTHash / NTLM**
NTHash is the hash format used by modern Windows OS us to store user and service passwords. The previouse version used by Windows is "LM", hence the name "NTLM".

**Exercise**
# /etc/shadow Hashes
The `/etc/shadow` file is used in Linux machines to store password hashes, aswell as information about the passwords, like date of last password change and expiration information. This file is usually only accessible by the root user.

**Unshadowing**
To crack the hashes we need to combine it with the `/etc/passwd` file so John can understand the data. We can use the built in tool "unshadow":
```
unshadow [path to passwd] [path to shadow] > output.txt
unshadow local_passwd local_shadow > unshadowed.txt
```
You can also just use the line you need from each file

**Cracking**
With the output from `unshadow` we can input them to John. Sometimes we need to specify the hash format, f.e. `--format=sha512crypt`
```
john --wordlist=/usr/share/wordlists/rockyou.txt (--format=sha512crypt) unshadowed.txt
```
# Single Crack Mode
In this mode John uses only the information provided by a username to try and find a possible password by changing letters and numbers contained in it. 

**GECOS**
GECOS is the name given to the individual fields found in `etc/shadow` seperated by a colon `;`. These are found in Unix OS and the information in them can be given to John to add to the wordlist it generates when using "Single Crack Mode".

**Using Single Crack Mode**
To crack a hash you need to prepend the hash with the username it belongs to in the hash file, f.e. `mike:1efee03cdcb96d90ad48ccc7b8666033`
```
john --single --format=[format] [path to file]
john --single --format=raw-sha256 hashes.txt
```

**Exercise**
# Custom Rules
When using the "Single Crack Mode" you can define custom rules regarding the creation of potential passwords and their structure.
Many corporations have a required level of password complexity, meaning that it may must include:
- Capital Letter
- Number
- Symbol

Most of the time most users will use predictable patterns for their passwords, the first letter will be capital and at the end will be a number followed by a symbol, f.e. "Password1!". So we could implement a custom rule for John that follows this structure.  

**Creating Costum Rules**
The custom rules are defined in the `/etc/john/john.config` file, or in `/opt/john/john.config` when you have installed John using `make` or a package manager. 
In the first line you define the name of your rule, which will be used to call the rule as an argument:
`List.Rules:RuleName`
You can then define the pattern using the modifiers:
- `Az`: Take the word and append a character from a defined set
- `A0`: Take the word and prepend a charcter from a defined set
- `c`: capitalize the character, at the corresponding position

After that you can define the set of letters, numbers and symbols you want to use by writting them inside `[]` and enclosing all with `""`:
- `[0-9]`: use the numbers 0-9
- `[0]`: only use 0
- `[A-z]`: use both upper and lowercase letters
- `[A-Z]`: only use uppercase letters
- `[a-z]`: only use lowercase letters
- `[a]`: only use a
- `[!§%@€]`: use the symbol "!§%@€"

For example:
```
[List.Rules:TestRules]
cAz"[0-9] [A-Z]"
```
and could then call it with the `--rule=ruleName` flag:
```
john --wordlist= [path to wordlist] --rule=TestRules [path to file]
```
# Cracking Password Protected Zip Files
To crack the password of a ZIP file we first have to use the Zip2John tool: 
```
zip2john [options] [zip file] > [output file]
zip2john zipfile.zip > zip_hash.txt
```
After that we can crack the password:
```
john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
```

**Exercise**
# Cracking Password Protected RAR Archives
To crack the password we first have to use the Rar2John tool
```
rar2john [rar archive] > [output file]
rar2john rarfile.rar > rar_hash.txt
```
We can then crack the password
```
john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
```

**Exercise**
# Cracking Password Protected SSH Keys 
To crack the password we first have to use the SSH2John tool; if you do not have it installed you can use 
`python /usr/share/john/ssh2john.py` instead:
```
ssh2john [id_rsa file] > [output file]
ssh2john id_rsa > id_rsa_hash.txt
(python /usr/share/john/ssh2john.py [id_rsa file] > [output file])
```
We can then crack the password
```
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
```

**Exercise**

# Further Resources
- [Hash format cheat sheet](https://pentestmonkey.net/cheat-sheet/john-the-ripper-hash-formats)
- [Comprehensice Guide](https://miloserdov.org/?p=4961) 