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