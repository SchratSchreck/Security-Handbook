# Introduction
Uploading files to a server is a integral part of the useage of the web, if it is uploading a (profile) picture, a report to a cloud storage or a project on Github, there are a number of applications for file uploading.
Being able to upload a file can also be a severe vulnerability, as when not implemented correctly it can lead to minor problems all the way up to *Remote Code Execution (RCE)*.
The possible attacks discussed here are:
- Overwriting existing files on a server
- Uploading and executing shells on a server
- Bypassing Client-Side filtering
- Bypassing various kinds of Server-Site filtering
- Fooling content type validation checks
# Methodology
As always ennumeration is the most important part and to find out the most about your target you can:
- Look at the page's source code to see if *client-side filtering* is applied
- Scan with a directory bruteforcer such as [Gobuster](https://github.com/OJ/gobuster) (install with  `sudo apt install gobuster`)
- Intercept upload requests with *[[Burp Suite]]*  
- Use [Wappalyser](https://www.wappalyzer.com/) to find more information about the website

After having a basic understanding how the website handles our input we can try to upload something to see what we can and can not upload.
If *client-side filtering* is active we can look at the source code and try to bypass it. 
In the case of *server-side filtering* we have to find out what filter is applied, upload a file and try something different based on the error message.
# Overwriting Existing Files
When a file is uploaded to a server some checks should be made to ensure no existing file is overwritten. It is common that a new name will be assigned to the file, either random or with the data and time it was uploaded. It can also be checked if the name of the file already exists on the server and a error message is returned.
It is very likely that in a real world scenario file permissions will prevent you from doing significant harm with this attack, but it is still worth to keep an eye out for in a pentest or bug hunting environment.

**Example**
We are visiting a website which displays a picture of a spaniel, which we want to overwrite.
1. We take a look at the source code of the website and discover that the picture is sourced from a file named "spaniel.jpg" inside a directory called "images"
2. We download a image, name it "spaniel.jpg" and upload it to the website
3. The picture we downloaded should now be displayed instead of the original

We have successfully overwritten the file, although in most cases you have to do more enummeration than look at the page's source code.

**Exercise**
1. We visit a website with a banner "File Overwriting" and two buttons "Select File" and "Upload", in the background we can see a picture of a montain
2. We look at the source code of the page and see: 
```
<img src="images/mountains.jpg" alt="">
``` 
3. We download or use some picture we already have, name it "mountains.jpg" and upload it
4. The text "Well done -- you overwrote the file!" is displayed along with the flag
# Remote Code Execution (RCE)
Uploading a payload, that allows RCE, via a upload vulnerability works by uploading a program that is written inthe same language as the back-end of the website or a language that the server understands (f.e. PHP, Python Django, Node.js). 
Most modern web frameworks are routed programmatically, meaning the routes are defined programmaticaly and not by being mapped to the file-system, which makes this method of attack more complicated and less likely.
There are two ways of achieving RCE when exploiting an upload vulnerability:
- **webshell**
- **reverse /bind shell**

A reverse shell is the ideal goal, however if there is a file length limit or a firewall prevents network-based shells, a web shell may be the only option.

**Examples**

**Web shell**
1. We visit a web site with an upload form
2. We do a *gobuster* scan and discover the directories "/uploads" and "/assets"
3. We upload an image, navigate to `/uploads` and see that our picture is placed there
4. Now we try to upload a simple webshell, which takes a parameter and executes it as a system command. In PHP the syntax is: 
```
<?php echo system($_GET["cmd"]);?>
```
5. We upload the the shell, calling it "webshell.php", and navigate to `/uploads` 
6. We should see our shell placed there

From here we can try to upgrade to a reverse shell or read the files form the system.

**Reverse shell**
The process of uploading a reverse shell is almost identical to that of a web shell
We will use the Pentest Monkey reverse shell, which comes by default with Kali Linux and can be downloaded [here](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php). (You have to change the IP address in line 49 to your own IP address)
1. Start a netcat listener with: 
```
nc -lvnp 1234
```
1. Upload the shell and navigate to `/uploads/shell.php`
2. The web site should not load properly, changing back to the listener we should see that the shell has connected to the listener
3. Now we can stabilise our shell or escalated our privileges

**Exercise**
# Filtering
There are two categories of filtering:
- *client-side* filtering: happens in your browser, easy to bypass
- *server-side* filtering: runs on the server, more difficult to bypass

**Extension Validation**
File extention filter check the file type to determin if the file is allowed to be uploaded by either:
- blacklist extensions: a list with extentions that are not allowed
- whitelist  extensions: a list with extension that are allowed

This type of filtering is used by MS Windows to identify file types.

**Flle Type Filtering**
Similar to *Extension Validation* this type of filtering checks if the content of the file are acceptable to upload

**1. Multipurpose Internet Mail Extension (MIME)**
MIME is used to identify files, originaly when transfered as an attachement over email, but also used over HTTP(S). MIME can be checked on the client or server side and are easy to bypass as it is based on the extension of the file. 
The filter is attached in the header of the request and has the format `type/subtype`
```
Content-Type: image/jpeg
```

**2. Magic Number Validation**
The magic number of a file is a string of bytes at the beggining (and end) of a file and identify the content of it. 
The magic number of a PNG file is:
```
89 50 4E 47 0D 0A 1A 0A
```

Unix systems use magic numbers to identify files.

**File Length Filtering**
This filter limits the length of a file that can be uploaded. This is in most cases not a problem, but if the form only accepts a file length of 2Kb then the shell we used earlier would be too large as it is 5.4 Kb.

**File Name Filtering**
This filter can work by checking if the given file name already exists in the system or to give them a random file name once uploaded. Additionally, file names should be sanatized meaning there should be no nullbytes, `\`, control characters such as `;` and unicode characters