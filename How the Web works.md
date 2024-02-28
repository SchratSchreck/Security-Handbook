
# How The Web Works

## DNS in detail

### What is DNS?

_Domain Name System (DNS)_ protocol provides a simple way of communicating with devices without having to remember their IP address. It does this by resolving hostnames like “tryhackme.com” to their IP address.

### Domain Hierachy  
  
**Top-Level Domain (TLD)**

A TLD is the most rigthhand part of a domain name, for example the “tryhackme.com” TLD is “.com”. The TLD can only use a-z 0-9 and hyphens. There are two types of TLDs:

- **Generic Top-Level Domain (gTLD):** meant to tell the user the purpose of the domain: “.org” for an organization, “.com” for commercial purposes, …
    
- **Country Code Top Level (ccTLD):** used for geographical purposes like “.de”, “.us”, …
    

**Second-Level Domain**

Taking “tryhackme.com” as an example “tryhackme” would be the Second-Level Domain. The Second-Level Domain is limited to 63 characters.

**Subdomain**

A subdomain sits on the left-hand side of the Second-Level Domain and is separated by a period. In “admin.tryhackme.com”, “admin” would be the subdomain. A subdomain has the same naming conventions as the Second-Level Domain. Multiple Subdomains can be split with periods to create longer names like “jupiter.servers.tryhackme.com”

### Record Types

There are multiple types of DNS records.

**A Record**

These records resolve to IPv4 addresses, for example 104.26.10.229.

**AAAA Record**

These records resolve to IPv6 addresses, for example 2606:4700:681a:be5

**CNAME Record**

These records resolve to a another domain name, for example “store.tryhackme.com” returns a CNAME record “shops.shopify,com”. Another DNS request would then be made to resolve “shops.shopify.com” to a IP address.

**MX Record**

These records resolve to the address of the server that handles the email for the domain you are querying. A MX record response to “tryhackme.com” would look like “alt1.aspmx.l.google.com”. These records also have a priority flag, which tells the client in which order to try the servers, in case the main server goes down.

**TXT Record**

These records are free text fields where any text-based data can be stored. One of the uses of these records are to list servers that are authorized to send emails on behalf of the domain. They can also be used to verify ownership of the domain name when signing up for a third party service.

### Making a Request

1. The computer first checks its local cache, when you request a domain name, to see if you recently looked up the address. If not a request to your Recursive DNS Server will be made.
    
2. A Recursive DNS Server is usually provided by your ISP, but can also chosen by you. This server also has a local cache, popular and heavily requested services (Google, Facebook,…) are commonly stored. If the requested domain is not found, a request is made to the root server.
    
3. The root servers are the DNS backbone of the internet; they redirect a request to the correct TLD server. If you requested “tryhackme.com” the root server will redirect you to the TLD server that deals with .com addresses.
    
4. The TLD server has a record where to find the authoritative server, also known as nameserver, to answer the DNS request. They are responsible tot store the DNS record for a domain name. The nameserver for “tryhackme.com” is “kip.ns.cloudflare.com” and “uma.ns.cloudflare.com”. Often there are multiple nameservers for a domain which act as a backup.
    
5. Depending on the record type, the DNS record will be send to the Recursive DNS server where a local copy is cached and relayed to the client that made the request. The DNS records also has a Time to Live (TTL).
    

## HTTP in detail

### What is HTTP(S)?

**What is HTTP (HyperText Transfer Protocol)?**

HTTP was developed by Tim Berners-Lee and his team between 1989-1991. It is the set of rules used for communicating with web servers for transmitting webpage data, whether HTML, images Videos, etc and is used whenever you view a website.

**What is HTTPS (HyperText Transfer Protocol Secure)?**

HTTPS is the secure version of HTTP, where data is encrypted which prevents people from seeing data you send and receive but also assure that you are communicating with the correct web server.

### Requests And Responses

**What is a URL (Uniform Resource Locator)?**

A URL is an instruction on how to access a resource on the internet. A URL with all its features used would look like this (not all features are used in every request):  
 ```http
 http://user:password@tryhackme.com:80/view-room?id=1#task3
``` 

- **Scheme:** (http) Instructs what protocol to use for accessing the resource such as HTTP, HTTPS, FTP (File Transfer Protocol)
    
- **User:** (user:password) Some services need authentication to log in, you can put a username and password into the URL to log in.
    
- **Host:** (tryhackme.com) Domain name or IP address of the server you want to access
    
- **Port:** (80) Port you want to connect to ( f.e. 80 for HTTP, 443 for HTTPS)
    
- **Path:** (view-room) File name or location of the resource you want to access
    
- **Query String:** (?id=1) Extra information that can be sent to the requested path. For example, /blog?id=1 would tell the blog path to access the blog article with the id of 1.
    
- **Fragment:** (#task3) Reference to a location on the actual page requested. Used for pages with long content where a part of it is directly linked.
    

**Making a Request**

A request is made up of:

- the request method
    
- the HTTP Protocol Version
    
- The page being requested
    

It is possible to make a request to a web server with just one line “GET/HTTP/1.1”.

Additional data is sent in headers, which contain extra information for the server.

**Example Request**  
  
```http
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```
- **Line 1:** The request is sending the GET method, request the home page with / and telling the server we are using HTTP version 1.1.
    
- **Line 2:** Telling the server we want the website tryhackme.com
    
- **Line 3:** Telling the server we are using FireFox version 87
    
- **Line 4:** Telling the web server that the web page that referred us to this one is “https://tryhackme.com”.
    
- **Line 5:** HTTP requests always end with a blank line to inform the web serber the request has finished.
    

**Example Response**
```http
HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
	<title>TryHackMe</title>
</heasd>
<body>
	Welcome To TryHackMe.com
</body>
</html>
```
  

- **Line 1:** The HTTP version the server is using (HTTP 1.1) followed by HTTP Status Code (“200 OK”), telling us the request was successful.
    
- **Line 2:** The web server software and version number.
    
- **Line 3:** Current date, time and timezone of the web server.
    
- **Line 4:** Content-Type header, telling the client what sort of information is going to be sent, HTML, images, videos , pdf, XML.
    
- **Line 5:** Content-Length, telling the client how long the response is. Allowing us to confirm is data is complete.
    
- **Line 6:** HTTP response contains a blank line confirming the end of the HTTP response.
    
- **Line 7-14:** The information that was requested, in this instance the homepage.
    

### HTTP Methods

HTTP methods allows the client to show their intended action when making a HTML request. The most common are covered here, although you will mostly deal with the GET and POST method.

- **GET Request**: Used for getting information from a web server.
    
- **POST Request:** Used for submitting data to a web server and creating new records.
    
- **PUT Request:** Used for submitting data to a web server to update information.
    
- **DELETE Request:** Used to delete information / records from a web server.
    

### HTTP Status Codes

When a HTTP server responds. The first line contains a status code informing the client of the outcome of their request and potentially how to handle it. These codes can be broken down into 5 ranges:

| Response Code | Meaning |
| :--: | :--: |
| **100-199 Information Response** | Telling the client the first part of their request was accepted and they should send the rest of their request; no longer very common |
| **200-299 Success** | Used to tell the client their request was successful |
| **300-399 Redirection** | Used to redirect the client’s request to another resource; can be a different web page or web site |
| **400-499 Client Errors** | Used to inform the client there was an error with their request |
| **500-599 Server Errors** | Reserved for errors on the server-side; usually indicating a major problem with the server |

**Common HTTP Status Codes**

Besides the many different HTTP status codes, applications can define their own. The most common HTTP codes are:

| Response Code | Meaning |
| :--: | :--: |
| **200 OK** | The request was completed successfully. |
| **201 Created** | A resource has been created ( new blog post, user, etc.) |
| **301 Moved Permanetly** | Redirects the client’s browser to a new webpage or tells the seatch engine the page was moved and it should look there. |
| **302 Found** | Similar to the above; only temporary and change in the future |
| **400 Bad Request** | Tells the browser something was wrong or missing in their request; could be that a certain parameter was expected but nor send by the client |
| **401 Not Authorised** | You must be authorized with the application to view the resource; commonly with a username and password |
| **403 Forbidden** | You do not have permission to view the resource whether logged in or not |
| **404 Page Not Found** | \|The page / resource does not exist |
| **405 Method Not Allowed** | The resource does not allow the method request; send a GET request when the resource expects a POST request |
| **500 Internal Service Error** | The server has encountered some error with your request and does not know how to handle it properly |
| **503 Service Unavailable** | The server con not handle the request, it is either overloaded or down for maintanance\| |

### Headers

Headers is additional data you can send to the web server, although not required it is difficult to view a website properly without them.

**Common Request Headers**

Theses are sent form the client (usually a browser) to the server.

- **Host:** some web server host multiple websites, by providing this header you can tell which you require, otherwise you will receive the default website.
    
- **User-Agent:** contains your browser software and version number; telling the software helps with formatting; some elements of HTML, JavaScript and CSS are only available in certain browsers.
    
- **Content-Length:** when sending data to a web server this header tells it how much data to expect so it can ensure nothing is missing.
    
- **Accept-Encoding:** Tells the web server what type of compression the browser supports so the data can be smaller for transmition.
    
- **Cookie:** Data send to the server to help remember your information.
    

**Common Response Handler**

Headers that are returned to the client from the server after a request

- **Set-Cookie:** Information stored and send back to the web server on each request
    
- **Cache-Contol:** How long should the content of the response be stored in the browsers cache before requesting again.
    
- **Content-Type:** Telling the client what type of data, i.e. HTML, CSS, JavaScript, Images, PDF, Video, etc. allowing the browser to know how to process the data.
    
- **Content-Encoding:** What method has been used to compress the data for transmission.
    

### Cookies

Cookies are small pieces of data which are stored on your computer to remind a server who you are, personal settings and other things. Servers need cookies for those things because HTTP is stateless, meaning it does not keep track of your previous requests. They are saved when the browser receives a “Set-Cookie” header from the web server and are then send back with every request you make. Cookies are most often used for website authentication, but the value of it is usually not a plain text but a token which is not easily humanly guessable.

## How Websites Work

When visiting a website, your browser makes a request to a web server, a computer dedicated to handle the request, asking for data a bout the page you are visiting, the server responds with that data and the browser uses it to show you the page.

The two major components that make a website:

- **Front End (Client-Site):** the way your browser renders a website
    
- **Back End (Server-Side):** the way a server processes the request and returns a response.
    

Websites are primarily created using:

- **HTML,** to build websites and define their structure
    
- **CSS,** to make websites look pretty by adding style options
    
- **JavaScript,** implementing complex features on pages using interactivity
    

### HTML

HyperText Markup Language (HTML) is the language websites are written in. Tags are the building blocks of HTML pages, telling the browser how to display them.

Below is a code snippet of a simple HTML document:
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Page Title</title>
	</head>
	<body>
		<h1>Example Heading</h1>
		<p>Example paragraph..</p>
	</body>
</html>
```  
  

- `<!DOCTYPE html>`: defines the page as a HTML5 document; helps with standardization
    
- `<html>`: root element of the page, all other elements come after this
    
- `<head>`: contains information about the website such as title
    
- `<body>`: defines the HTML document’s body; only content inside the body are shown in the browser
    
- `<h1>`: defines a large heading
    
- `<p>` : defines a paragraph
    
- other tags are for example `<button>` or  `<img>` for images
    

Tags can contain attributes such as class attributes used to style an element (e.g. make the tag a different color) `<p class= “bold-text”>`; or the src attribute used to specify the location of an image "`<img src=”img/cat.jpg"` . An element can have multiple attributes each with their own purposes, e.g. `<p attribute1 =”value1” attribute2=”value2”>` .

Elements can also have an id attribute (<p id “example”>) which is unique to that element and used for styling and identify it by JavaScript.

To view the source code of a page right-click and select “View / Show Page Source Code”.

### JavaScript

JavaScript allows web pages to be interactive and control the functionality of it. Without JavaScript a website would be static and without interactive elements, such as a button that changes its style when pressed or animation.

JavaScript is added in the source code of a page either within `<script>` tags or included remotely with the src attribute:  
```html 
<script src="/location/of/javascript_fle.ja">
</script>
```

The following code finds a HTML element with the id “demo” and changes its content to “Hack the Planet”
```JavaScript
document.getElementById("demo").innerHTML = "Hack the Planet";
```  

The next code changes the text of a button when the button is pressed. Another event would be “onhover”.
```html
<button
onclick='document.getElementById("demo").innerHTML = "Button Clicked";'>Click Me!</button>
``` 
### Sensitive Data Exposure

Sensitive Data Exposure occurs when a website does not properly protect (or remove) sensitive clear-text information to the end-user; usually in the site’s source code. A developer may forgot to remove login credentials or links to private parts of the website. Whenever assessing a website for security issues review the page source code.
```html
<!DOCTYPE html>
<html>
	<head>
		<title>Fake Website</title>
	</head>
	<body>
		<form>
			<input type='text' name='username'>
			<input type= 'password' name='password'>
			<button>Login</button>
			<!-- TODO remove test credentials admin:password123 -->
		</form>
	</body>
</html>
```  
  
### HTML Injection

HTML injection is a vulnerability that occurs when unfiltered user input, HTML or JavaScript code, is displayed on the page. The general rule is to never trust user input and sanatise everything the user enters before using it in the JavaScript function.

## Putting It All Together

To summarize what you have learned. When requesting a website, the browser first uses DNS to find the IP of the server it needs to communicate with. Using HTTP the computer communicates with the server, which returns HTML, JavaScript, CSS, Images, etc. and is then formatted by the browser to display the website.

### Other Components

**Load Balancer**

Load balancers ensure that web application with high traffic or a need for high availability can handle the load and provide failover if a server becomes unresponsive. The load balancer will first receive the request and than forward it to one of the multiple servers behind the application. It uses different algorithms to decide which server is the best to deal with the request, examples are round-robin, which sends it to each server in turn, or weighted, checking how many requests a server has and sends the request to the least busy.

Health checks are also performed by load balancers, checking if the server is running correctly and if not it will stop sending traffic to the server until it responds appropriately again.

**Content Delivery Networks (CDN)**

A Content Delivery Network allows you to host static files such as JavaScript, CSS, Videos, Images from your website across thousands of servers across the world. When a user requests one of those files the CDN locates the neatest server and sends the request there and not potentially the other side of the world.

**Databases**

Databases are used to store and recall data belonging to a website. They can range from a plain text file to clusters of multiple servers. Common databases are MySQL, MSSQL, MongoDB, GraphQL, Postgres and more.

**Web Application Firewall (WAF)**

A WAF analyses web requests for common attack techniques, hacking or DoS, and if the request comes from a real browser or a bot, before sending it to the server. It also checks if there is an excessive amount of traffic from a IP address, and if so the requests will be blocked and not sent to the webserver.

### How Web Servers work
#Web-Server

What is a Web Server?

Web servers are software that listen to incoming connections and utilises HTTP to deliver its content to the clients. The mos common web server software are Apache , Nginx, IIS and NodeJS. Servers store their files in their root directory, defined in its settings. Nginx and Apach have the default root location /var/www/html; Linux and IIS have C:\inetpub\wwwroot

Virtual Hosts

Web server can host multiple websites with different domain names, this is achieved with the use of text based configuration files called virtual hosts. The web server checks the hostname in the HTTP requests and compares it to its virtual hosts, if no match is found default website will be presented.

Virtual hosts can have their root directory on different locations on the hard drive. For example, one.com could be mapped to /var/www/website_one, and two.com to /var/www/website_two.

Static Vs Dynamic Content

Static content never changes examples are pictures, JavaScript, CSS, etc. but can also include HTML. These files are served directly from the server with no changes made to them.

Dynamic content can change with different requests. A blog, for example, will present on its homepage the latest entries, when a new one is created the homepage will be updated with the new entry. Another example is a search page, which will display different results depending on the word you search.

Backend

Everything you can see in a browser is called the Frontend, everything else is called the Backend. In the Backend different languages are used to interact with databases, call external services, process user data and much more. Examples for languages are PHP, Python, Ruby, NodeJS, Perl and many more.

A basic PHP example is the request for the website “http://example.com/index.php?name=adam”

If index.php was built like this:

`<html><body> Hello <?php echo $_GET[“name”]; ?> </body></html>`

The output to the client would be

`<html><body> Hello adam</body></html>`

Notice that the client does not see any PHP code, because it is on the Backend. This interactivity opens a lot of security issues for web application that were not created securely.