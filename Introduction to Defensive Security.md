# Introduction to Defensive Security

## Intro to Digital Forensic

Digital Forensics is the application of computer science to investigate crimes related to digital systems and digital evidence for legal reasons. Digital devices at a crime scene could be computers, laptops, digital cameras, music players, smartphones; CDs, DVDs, USB flash drives and external storage. A few questions arise:

- How should digital evidence be collected? What procedures should be followed if a device is turned on?
    
- How to transfer the digital evidence and what best practices to follow when moving computers?
    
- How to analyze the digital evidence? A device storage can range from a few gigabytes to several terabytes.
    

Digital forensics is used in two types of investigations:

1. **Public-sector investigations:** Investigations carried out by government and law enforcement as a part of a crime or civil investigation.
    
2. **Private-sector investigation:** Investigations carried out by corporate bodies by assigning a private investigator, in-house or outsourced, triggered by corporate policy violations.
    

### Digital Forensics Process

The steps after arriving at a crime scene as a digital forensic investigator:

1. **Acquire the evidence:** Collect the digital evidence (laptops, storage devices, digital cameras, …). When a device is turned on it requires special handling.
    
2. **Establish a chain of custody:** Fill out the related form appropriately, to ensure that only authorized investigators have access to the evidence and it is not tempered with.
    
3. **Place the evidence in a secure container:** This ensures that the evidence is not damaged and devices like smartphones do not have access to the network, so they do not get wiped remotely.
    
4. **Transport the evidence to your digital forensic lab**
    

At the lab, the process goes as followed:

1. **Retrieve the digital evidence from the secure container**
    
2. **Create a forensic copy of the evidence:** Advanced software is required to avoid modifying the original data.
    
3. **Return the evidence to the secure container:** Work on the copy, if it gets damaged create a new one.
    
4. **Start processing the copy on your forensics workstation.**


Included in digital forensics is:

- **Proper search authority:** Without proper legal authority, a investigator cannot commence
    
- **Chain of custody:** Keeps track of who was holding the evidence.
    
- **Validation with mathematics:** With a hash function we can confirm that the file has not been modified.
    
- **Use of validation tools:** Only use validated tools, to ensure that they work correctly.
    
- **Repeatability:** The findings should be repeatable, with the proper skills and tools
    
- **Reporting:** The investigation is concluded with a report that shows what evidence was discovered
    

### Practical Example of Digital Forensic

Our cat Gado was kidnapped and the kidnapper sent us a document and a image.

**Document Metadata**

When creating a text file, the OS saves some metadata and editors like MS Word keeps much information in the file’s metadata. Metadata is data like creation date and last modification date. Depending on the PDF writer used most metadata maintains when exporting the text file to PDF.

To see the metadata we use the program “pdfinfo” if it is not installed on Kali Linux you can install it with “sudo apt install poppler-utils”. 


Photo EXIF Data

EXIF stands for ”Exchangeable Image File Format”, a standard for saving metadata to image files. In every photo shot with a smartphone or digital camera metadata is embedded into it. Metadata such as:

- Camera / Smartphone model
    
- Data and time of image capture
    
- Photo settings, focal length, aperture, shutter speed and ISO settings
    

In the case of a smartphone, because it has a GPS sensor, the GPS coordinates of where the photo was taken are also embedded into it.

A tool to read the EXIF data is “exiftool”, which can read and write metadata in types such as JPEG images. If exiftool is not installed on Kali Linux you can do that with “sudo apt install libimage-exiftool-perl”.

Scrolling through the data we eventually find the creation data and the GPS location.
51 deg 30' 51.90" N, 0 deg 5' 38.73" W

After entering the GPS location into Google maps we can find the place where the photo was taken.
## Security Operations

### Introduction

A _Security Operations Center (SOC)_ is a team of security professionals monitoring a network and systems 24 hours a day, seven days a week. Their Purpose is:

- **Find vulnerabilities on the network:** vulnerabilities are weaknesses that can be exploited by an attacker. These can be inside a software on any device in the network.
    
- **Detect unauthorized activity:** A attacker could discover the username and password of a employee and use it to log into the company’s system. A clue for this can be geographical location.
    
- **Discover policy violations:** A _security policy_ is a set of rules and procedures to protect against security threats and ensure compliance. A violation could be downloading pirated media or sending confidential files insecurely.
    
- **Detect intrusion:** _Intrusions_ could be an attacker exploiting a web application or a user visits a malicious site and infects their computer
    
- **Support with the incident response:** An _incident_ can be an observation, a policy violation, intrusion attempt ot a major breach. The SOC supports the incident response team handling the situation.
    
### Elements of Security Operations

**Data Sources**

The some sources to monitor a network for intrusions and malicious behavior:

- **Server logs:** There are many types of servers, mail servers, web server and domain controller on MS Windows networks. Logs contain information about the activities on a network, like un- / successful login attempts and many more.
    
- **DNS activity:** _Domain Name System (DNS)_ is the protocol for converting domain names, “tryhackme.com”, to an IP address, 10.3.13.37 and other domain name related queries. If someone tries to browse tryhackme.com the DNS server has to resolve it and can log the DNS query to monitoring.
    
- **Firewall logs:** Firewalls control network traffic by blocking or allowing pakets to enter a network. Firewall logs contain what packets passed or tried to pass.
    
- **DHCP:** _Dynamic Host Configuration Protocol (DHCP)_ assigns an IP address to the systems connecting to a network. DHCP automatically provides a device with the network settings, if you can join a network without manual configuration.
    

A SOC might use a _Security Information and Event Management (SIEM)_ system, which aggregates the data from the different sources so the data can be efficiently correlated to attacks.

**SOC Services**

Reactive services are tasks initiated after a intrusion or malicious event is detected. Some examples are:

- **Monitor security posture:** The primary role of a SOC. Monitoring the network and systems for security alerts ,notifications and responding to them.
    
- **Vulnerability management:** Finding vulnerabilities and patching them. The SOC can assist with this task but not necessarily execute it.
    
- **Malware analysis:** Basic analysis of malware in a controlled environment, by the SOC. For advanced analysis it is send to a dedicated team.
    
- **Intrusion detection:** _Intrusion detection system (IDS)_ is used to detect intrusions and suspicious packets. The SOC maintains this system, monitors its alerts and reviews its logs.
    
- **Reporting:** It is essential to report incidents and alarms to ensure workflow and support compliance requirements.
    

Proactive services are tasks handled by the SOC without an indicator of an intrusion. Some examples are:

- **Network security monitoring (NSM):** Monitoring the network data and analyzing traffic to detect signs of intrusions.
    
- **Threat hunting:** The SOC assumes and intrusion already happened and see if they can confirm this assumption.
    
- **Threat Intelligence:** Learning about potential adversaries, their tactics and techniques to improve company defenses. The purpose if to establish a _threat-informed defence_.
    

Cyber security training is also included in the services of the SOC.

**Example Scenario**

A SOC analyst monitors the network and manages the logs. While monitoring the network traffic the analyst notices a DNS query repeating every exact 60 seconds. The analyst checks the source of the DNS query, identifies the cause to be a laptop, isolates it and inspects it for infection. They discover a process (program) using DNS to communicate with a malicious server. After reviewing the computer logs they find that it was infected after visiting a malicious website. The laptop is cleaned and a threat hunting starts to ensure no other laptops are infected.

## Practical Example of SOC

A _firewall_ is a device that inspects packets entering or leaving a network or system. The most basic firewalls inspect:

- **Source and destination IP addresses:** _IP address_ is a logical address that is needed to communicate over the internet. An analogy is the postal address.
    
- **Source and destination port numbers:** In addition to an IP each program on the computer needs a _port number_ to communicate over the internet. A analogy would be similar to a room number.

