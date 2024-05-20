The [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/#) is a insecure web application which can be used as a CTF playground. The vulnerabilities in the shop encompasses the [[OWASP Top 10-2021]] vulnerabilities and many more.

# Looking around and Reconnaisance
It is important to look around so you can discover usefull information.

**The Administrators Email Address**
Clicking on the "Apple Juice" product we discover that it has a review by the admin, which displays the email. 
```
admin@juice-sh.op
```

**Parameter used for searching**
By clicking on the magnifying glass in the top right corner, a search bar will pop up. We enter some input and look at the URL, which has been updated with our input and we can see the parameter `/#/search?`
```
http://10.10.24.12/#/search?q=orange
```

**References by Users**
A user named "jim" reviewed the "Green Smoothie" withthe comment "Fresh out of a replicator.". After we google "Replicator" we discover that it is a machine out of the TV show "Star Trek" 
# Injection attacks
[[OWASP Top 10-2021#3. Injection|Injection]] is number 3 on the [[OWASP Top 10-2021]] and while there are different types of injection attacks we will focus on SQL Injection.

**Logging in the admin account**
1. We start *Burp Suite* and go to the login page
2. We enter some text and press "Enter"
3. We switch to *Burp Suite* and modify the email and password field:
```
{"email":"' or 1=1--"},{"password"="a"}
{"email":"admin@juice-sh.op"},{"password"="a"}
```

# [Broken Authentication](https://owasp.org/www-project-top-ten/2017/A2_2017-Broken_Authentication.html)
For this attack we will use the *Intruder* in *Burps Suite*.

**Bruteforcing the Admins password**
1. We again capture a login request but we will send it to the *Intruder*
2. We switch to the *Intruder*, go to the "password" field and select the "Clear §" button
3. Place `"§§"` in the password field
4. Navigate to the "Payloads" tab and load the " best1050.txt" list with potential passwords (got to "/usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt" or install with `apt-get install seclists`)
5. Start the attack and filter the requests by status, as a failed attempt receives the status code "401 Unauthorized" a successfull attempt the code "200 OK"
6. After a succsessful attempt with the payload "admin123", log into the account with it

**Reseting Jim's password**
1. After entering Jim's email (jim@juice-sh.op), his security question is displayed "Your eldest siblings middle name?". 
2. Scince Jim made a reference to "Star Trrek" we search for "Jim Star Trek" and go to the Wikipedia page for "James T. Kirk".
3. We discover the character's brother has the middle name "Samuel"
4. We enter the name and are now able to set a weak password

# [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A3-Sensitive_Data_Exposure) 
**Accessing Confidential Documents**
1. Navigate to the "About Us" page and hover over the "Check terms of use"
2. Notice that the link is directed to "http://10.10.69.231/ftp/legal.md"
3. Go to "http://10.10.69.231/ftp/" and look at the files
4. Download "acquisitions.md" and get the flag

**Downloading the Backup files**
In the `/ftp` folder is a file called "package.json.bak", if we try to download it we are given a "403 error code", which says that we only can download `.md` and `.pdf` files.
To circumvent this we will use a *Posion Null Byte*, by appending `%2500.md` to the url
```
http://10.10.69.231/ftp/package.json.bak%2500.md
```
Now we can download the backup file

# [[OWASP Top 10-2021#[1. Broken Access Control](https //owasp.org/Top10/A01_2021-Broken_Access_Control/)|Broken Access Control]] 
**Accessing the administration page**
1. Open the *Debugger* on your web browser by pressing `F12` on your keyboard or looking in the tools section of the browser
2. Refresh the page and look out for a file named "main-es2015.js"
3. Click the `{}` button to format it and search for the word "admin", specifficaly "path: administrator"
4. This hints at a page called `/#/administrator`, log into the admin account and visit this web page

**View another user's shopping cart**
1. Start *Burp Suite* log into the admin account and go to "Your Basket"
2. Forward each request until you see " GET /rest/basket/1 HTTP/1.1"
3. Change the number after `basket/` to "2", to get the basket of the user with the id "2"

# [Cross-Site Scripting (XSS)](https://owasp.org/www-project-top-ten/OWASP_Top_Ten_2017/Top_10-2017_A7-Cross-Site_Scripting_(XSS)) 
There are three major types of  XSS attacks:

| Type                                      | Description                                                                                      |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Document Object Model-based (DOM) XSS** | use the `<script></script>` HTML tag to execute malicious javascript                             |
| **Persistent (Server-side) XSS**          | javascript is executed when the page is loaded, often occurs when user uploads are not sanatized |
| **Reflected (Client-side) XSS**           | javascript is run on the client-side, often occurs when user search data is not sanatized        |
**Perform a DOM XSS**
We will use the iframe element, a common HTML element, with a javascript alert tag:
```
<iframe src="javascript:alert(`xss`)">
```
When we input this in the search bar a alert will be triggered with the text "XSS".
This type of XSS is called *Cross Frame Scripting (XFS)*, a common form of detecting XSS vulnerabilities within an application.

The search bar will send a request to the server, which will return the related data, but without correct input sanitation we can perform this XSS attack.

**Perform a persitstent XSS**
1. Start *Burp Suite*, log into the admin account and navigate to the "Last Login" page
2. Logout and make sure that the request is captured
3. Switch to *Burp Suite* and to the "Header" tab
4. Add a new header with the name "True-Client-IP" and the value 
```
<iframe src="javascript:alert(`xss`)">
```
5. Forward the request and then sign back in and visit the page again
6. It should now display the alert massage

**Perform a reflected XSS**
1. Login the admin account and navigate to "Order History"
2. Click on the truck icon, this will bring you to the track result page and you will see that there is an id paired with the order inside the URL. 
```
192.168.1.101/#/track-result?id=5267-f73dcd000abcc353
```
3. We will replace the id with the payload 
```
192.168.1.101/#/track-result?id=<iframe src="javascript:alert(`xss`)">
```
4.Submit the URL and refresh the page the alert should then be displayed.

The server will have a lookup table or database for each ID, if the id parameter is not sanatized before it is send to the server we are able to perform an XSS attack.

**Score Board**
Under the `/#/score-board` section you can see a list of all challanges in the shop.