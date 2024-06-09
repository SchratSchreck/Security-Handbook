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
# Cracking Basic Hashes
**Basic Syntax**
The basic syntax for John is `john [options] [path-to-hash]`

**Automatic Cracking**
John has the option to automaticaly detect the type of hash you want to crack, but this can be unrelyable.
`john --wordlist=[path to wordlist] [path to hash]`:
`john --wordlist= /usr/share/wordlists/rockyou.txt hash_to_crack.txt`

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