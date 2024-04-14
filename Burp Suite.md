# Basics
*Burp Suite* is a Java-based framework that allows the capture and manipulation of [[How the Web works#HTTP in detail|HTTP/HTTPS]] traffic between a browser and a server. 

# Features
- **Proxy**: Enables the inception and modification of requests and responses from web applications
- **Repeater**: Allows the capture, modification adn resending of the same request multiple times. This is useful when crafting payloads by trial and error for *SQL Injection*.
- **Intruder**: Allows to spray the endpoints with requests; useful for brute-force attacks or fuzzing
- **Decoder**: Decodes captured information or encode payloads 
- **Comparer**: Allows comparing data at either word or byte level
- **Sequencer**: Employed when assessing the randomness of tokens, like session cookie value or other randomly generated data. If the algorithm lacks secure randomness, it can expose vulnerablities for attacks.

Burp Suite also allows the development of extensions written in Java, Python or Ruby. With the *Burp Suite Extender* module you can load extensions into the framework and the *BApp Store* enables you to download third-party modules.
# Introduction to the Burp Proxy
- **Intercepting Requests**: Reqeuests made trhough the Proxy will be intercpted and held back allowing the user to forward, drop edit or send the request to another module.
- **Capture and Logging**: Requests are captured and logged by default, which is useful for analysis and reviewing.
- **WebSocket Support**: WebSocket communication is also captured and logged
- **Logs and History**: Captured requests can be viewed in the "HTTP history" and "WebSocket history" sub-tabs

**Settings**
- **Response Interception**: By default server responses are not intercepted, to enable this click on the checkbox and the defined rules "Was intercepted"
- **Match and Replace**: This section in the settings enables the use of regular expressions to modify requests, allowing dynamic changes, such as modifying user agent or cookies.

## Connecting through the Proxy (FoxyProxy)
You need to configure your web browser to redirect traffic to Burp Suite. This can be done with the FoxyProxy web browser extension.

1. Download the [FoxyProxy](https://getfoxyproxy.org/downloads/) extension
2. Open "Options" and click "Add" 
3. Enter the values, for example:
	   - Title: "Burp"
	- Proxy IP: own IP
	- Port: 8080
4. Save the configuration and activate FoxyProxy by clicking the icon in your browser
5. Enable interception in Burp Suite in the "Proxy" tab

## Proxying HTTPS
You might encounter an issue when navigating to sites TLS enabled (HTTPS sites), because your browser does not trust the certificate presented by Burp Suite.
To resolve this issue you have to manually add the PortSwigger CA certificate to your browser:
1. Download the certificate from [PortSwigger](https://portswigger.net/burp/documentation/desktop/external-browser-config/certificate)
2. Access your browsers certificate settings
3. Add/import the certificate to your browser
4. Enable the option to trust the CA to identify websites

## XSS with Burp Suite
1. We try typing `<script>alert("Succ3ssful XSS")</script>` into the "contact email" field of a support form, but it prevents the use of sepical characters not allowed in email addresses
2. Activate the Burp Suite proxy, enter some email, f.e. "pentester@example.thu" and some text in the text field
3. The request should be intercepted by the proxy; change the email to the payload `<script>alert("Succ3ssful XSS")</script>` , select it and encode it with `Ctrl+U` 
4. Press "Forward"; it should bypass the client-side filter and the website should display an alert box with the message "Succ3ssful XSS"