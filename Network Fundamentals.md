#Pre-Security
# Network Fundamentals

## What is Networking?

Networks are things connected. In computing these things are technological devices, like smartphones, laptops, security cameras, traffic lights and more. A network can be formed by anywhere from 2 devices to billions.

### What is the Internet?

The internet is a giant network consisting of smaller networks, called private networks, joined together. The networks connecting the smaller networks are called public networks.

The first iteration of the Internet was within the ARPANET project in the late 1960s, funded by the US military. The World Wide Web was the creation of Tim Berners-Lee in 1989 and was the invention of the internet as we know it, by being used as a repository for storing and sharing information.

### Identifying Devices on a Network

To communicate over the internet devices need to be identifiable on a network. To ensure this every device has:

- An IP addresss
    
- A Media Access Control (MAC) address
    

**IP Addresses**

An _Internet Protocal (IP)_ address is used to identify a device on a network. The IP can be associated with another device without the IP changing, but can not be active simultaneously within the network.

An IP is a set of numbers divided into four octets. The number is calculated through IP addressing & subnetting.

Because a device can be on a private or public network, they have a public and private IP address. The public IP identifies the device on the internet, the private a device among other devices. Two devices in a private network use their private IPs to communicate with each other. Any data sent over the internet by them will be identified by the public IP. The public IP is given to you by your _Internet Service Provider (ISP)_ at a monthly fee.

The problem that there are more devices on the internet than IPv4 addresses (4.29 billion), IPv6 tackles this problem. Although more daunting, the benefits are 340 trillion-plus IP addresses and more efficiency due to new methodologies.

**MAC Addresses**

The _Media Access Control_ address is a twelve character hexadecimal number, assigned to to the physical network interface microchip found on the motherboard. The first six characters of the MAC address represent the company that made the chip, the last six is an unique number.  

  
MAC addresses can be faked or “spoofed”. This occurs when a device pretends to identify as another device by using its MAC address. A scenario could be that a firewall is configured to allow any traffic going to and from the A´MAC of the administrator. Spoofing this MAC lets any traffic from this device pass the firewall.

## Intro to LAN

### Introducing LAN Topologies

When talking about “topology”, it is referring to the design or look of a network.

**Star Topology**

he devices in this network are individually connected to a central device such as a switch or a hub. Any information sent to a device in the network is sent via the central device.

**Advantages:**

- more scalable than other topologies
    
- less prone to failure
    

**Disadvantages:**

- more cable and network equipment is required
    
- the bigger the network the more maintenance is needed
    
- if the central device fails the network does not work anymore
    

**Bus Topology:**  

The network relies on a single connection, the backbone cable. It becomes easily bottle necked if devices are simultaneously requesting data.

**Advantages:**

- easy to set up and cost-efficient
    

**Disadvantages:**

- prone to bottlenecking if devices request data simultaneously
    
- difficult to identify which device has issues with data
    
- little redundancy in case of failures
    

**Ring Topology**

The devices are all connected directly to each other. Data is send in one direction until it has reached its destination, devices along the loop forward the data to the next. A device will only forward data if it does not have any to send itself. If the device needs to send data it will send its own first and than forward data from another device.

**Advantages:**

- easy to troubleshoot any faults
    
- less prone to bottlenecks
    

**Disadvantages:**

- not an efficient way of data transportation
    
- if a cable or device is broken the network does not function
    

**What is a Switch?**

Switches are devices to aggregate multiple devices, computer, printers and other noetworking-capable devices using ehternet. Switches can have 4, 8, 16, 24, 32 and 64 ports for devices to plug into. They are also more efficient than hubs/repeaters and keep track what device id connected to which port. So instead of repeating a packet to each port, like a hub, it sends it to the intended target, reducing network traffic.

Switches can also be connected to Routers, which increases the redundancy (reliability) of a network by adding multiple paths for data to take.

**What is a Router?**

A Router connects networks, by using routing. Routing is the label of the process of data travelling across networks, which involves creating a path between networks.
### A Primer on Subnetting

Subnetting refers to splitting a network into smaller ones. This is achieved by splitting up the number of hosts that can fit into a network, represented by a number called a subnet mask. A subnet mask is represented by as a number of four bytes (1 byte = 32bit), ranging from 0 to 255.

Subnets use IP’s in three different ways:

- **Identify the network address:** The _Network Address_ identifies the start of the network and its existence.
    
Example: 192.168.1.0

- **Identify the host address:** The Host Address identifies a device on the subnet.
    
Example: 192.168.1.100

- **Identify the default gateway:** The _Default Gateway_ address is assigned to a device that is capable of sending information to another network (not to a device in 192.168.1.0). It can have any address but usually have either the first or last host address (.1 or .254).
    
    Example: 192.168.1.254
    

Subnetting provides a range of benefits:

- Efficiency
    
- Security
    
- Full control
    

The security is allows to separate a network; for example in a cafe one for employees, cash register, etc and one for public hotspot.

### The ARP Protocol

The _Address Resolution Protocol_ (ARP) allows devices to identify themselves on a network, by associating a IP with a MAC address. Each device will store an ARP table containing a mapping of which IP has which MAC. When communicating with another they will send a broadcast to the entire network searching for the specific device

**How does ARP Work?**

Each device has a cache in which the ARP table is stored. To map the IP and MAC the ARP protocol sends two messages:

1. **ARP Request:** broadcasted to ask if the device’s MAC matches the requested IP
    
2. **ARP Reply:** is returned to the initial request the initial device will store the mapping as an ARP entry.
    
### The DHCP Protocol

The _Dynamic Host Configure Protocol (DHCP)_ assigns a device an IP address when it enters the network and it has not been manually assigned an IP.

The steps for the assignment are :

1. **DHCP Discover:** the device broadcasts a request to see if any DHCP servers are on the network
    
2. **DHCP Offer:** the DHCP server replies with an IP the device could use.
    
3. **DHCP Request:** the device sends a reply confirming the use of the offered IP
    
4. **DHCP ACK:** the DHCP server send a reply acknowledging that the device can start using the IP
    
## OSI Model

### What is the OSI Model?

The _Open System Interconnection (OSI)_ model is the fundamental model in networking, providing a framework which dictates who devices will send, receive and interpret data.
Although not used in real-world networking it is excellent for learning and understanding the concepts of networking.

Data sent across a network that follows the OSI model can be understood by other devices with different functions and designs.

The OSI model consists of 7 layers, each with its own responsibilities. At every layer new information is added in a process called *Encapsulation* and when received are *De-encapsulated*, meaning the added data is then striped off layer after layer.

The layers are:
  
### Layer 7 – Application

In this layer protocols and rules are in place to determine how users should interact with data sent or received. User interact with a _Graphical User Interface (GUI)_ provided by everyday applications and provides a interface apps can use to transmitt their data.

The *DNS* protocol, which translates domain names to IPs is also a protocol of this layer.

### Layer 6 – Presentation

This layer translates data into a standardised format, acting as a translator for data to and from the application layer. It also handles encryption(*HTTPS*), compression and other transformations to data A computer receiving data in a specific format will still understand data sent in another format.
### Layer 5 – Session

When data has been translated and formatted (Presentation layer), the session layer will create a connection to the other device. When a connection is established a session is created. The computers are synchronized before dividing the data in small _packets_. If the connection is lost only the _packets_ that were not sent must be sent again instead of the whole data.

Sessions are unique, data can not travel over different sessions, but only across each session.

### Layer 4 – Transportation

This layer is vital for transmitting data between devices and can be done following two protocols, TCP and UDP.
The data (called Segments or Datagrams) added here during Encapsulation contains, among other things, data specific to the choosen protocol. 

**Transmission Control Protocol (TCP)**

It is used for file sharing, internet browsing, sending an email and other services that require data to be accurate and complete.

Advantages

- Guarantees reliable transmission of data and the other device receives it
    
- Synchronizes two devices preventing them being flooded with data
    
- More processes for reliability
    

Disadvantages

- Requires a reliable connection, if one small part of a chunk of data is not received the chunk can not be used
    
- A slow connection can bottleneck other devices as the connection will be reserved on the receiving computer
    
- Slower than UDP
    

**User Datagram Ptotocol** **(UDP)**

It is used when small pieces of data is sent and it is not important that every piece is reliably transmitted. ARP, DHCP use this protocol and it is also used when sending large files such as in video streaming, where lost pieces in data only result in pixelation.

Advantages:

- faster than TCP
    
- leaves the application layer control how quickly packets are sent
    
- does not reserve a continuous connection on a device
    

Disadvantages:

- no guarantee that data is reliably transmitted
    
- flexible to software developers
    
- unstable connection can result in a terrible experience for the user
    

### Layer 3 – Network

On this layer routing and re-assembly of data takes place. Routing determines the optimal path on which the data should be sent, using protocols such as _Open Shortest Path First (OSPF)_ and _Routing Information_ _Protocol (RIP)._ At this layer Logical addressing (IPs) are used. Routers and other devices are known as Layer 3 devices. Factors which decide what route to take are:
The header added during Encapsulation contains source and destination IP address. 
- What path has the least amounts of devices?
    
- What path is the most reliable?
    
- Which path has the faster physical connection (copper or fibre)?
    
Data added here during *Encapsulation* are called Packets
### Layer 2 – Data link

This layer focuses on the physical addressing, by adding the physical _Media Access Control (MAC)_ address, of the receiving endpoint, to the packet received from layer 3. MAC addresses are set by the manufacturer of _Network Interface Card (NIC),_ which is inside every network-enabled computer, and can not be changed, but spoofed.

It is also responsible to present the data in a form suitable for transmission and check received data to make sure it has not been corrupted, by adding a trailer at the end of the transmission during *Encapsulation*.  
 
### Layer 1 – Physical

This layer translates the binary data into (electrical) signals transmiting them and translate incoming signals back into binary. 
Ethernet cables are used to transfer data in binary.

## Packets and Frames

### What are Packets and Frames

_Packets_ and _frames_ are small pieces of data, when combined make a larger piece of data. A _frame_ is at layer 2, data link layer, in the OSI model, so there are no information as IP addresses. A packet however does have IP addresses. This is like putting an envelope in another one before sending it away.

_Packets_ are used to efficiently communicate data across networking devices. The data is transmitted in small packets and then reconstructed. Different types of packets have different structures, but all have standards and protocols on how devices handle them.

A packet using the Internet Protocol has a set of headers, containing additional pieces of information.

- **Time to Live:** sets an expiry timer for the packet to not clog up the network
    
- **Checksum:** provides integrity checking for protocols like TCP/IP. If data is changed, the value will be different.
    
- **Source Address:** IP address of the device that send the packet
    
- **Destination Address:** IP address of the device that is supposed to receive the packet
    

## TCP/IP Model

The _Transmission Control Protocol (TCP)_ is a protocol used in networking (OSI Layer 4) and controls the flow of data. It consists of four layers, similar to the OSI model:

- Application
    
- Transport
    
- Internet
    
- Network Interface
    
Information is added to each layer of TCP as the packet traverses it, called encapsulation with the reverse being decapsulation.
The *Internet Protocol* controls how packets are addressed and send.
This model is the basis for real-world networking.
### TCP Three-Way-Handshake
TCP is connection based, meaning it must establish a connection between devices before data is sent. Because of this TCP guarantees that data that is send will be received. To establish a connection is performs a process called the *Three-way handshake*.
  
1. **SYN-Client:** initial packet sent by a client containing its _Initial Sequence Number (ISN)_
    
2. **SYN/ACK-Server:** sent by the server to acknowledge the synchronization attempt containing the ISN of the server and an acknowledgment of the clients ISN.
    
3. **ACK-Client:** To complete the Three-way handshake the client sends an ACK packet containing the ISN of the server and data that is the clients ISN+1. The ACK packet can be used by both the server and the client.
    

A three-way handshake is also performed when closing the connection:
  
Besides these packets there are also the DATA and RST (abruptly ends communication) packets.

TCP packets contain information called headers, which are added from encapsulation:

- **Source Port:** value of the port from which the packet was send.(This value is chosen randomly out of the ports 0 – 65535 that are not used already)
    
- **Destination Port:** Value of the port an application or service is running on. These are not chosen randomly.
    
- **Source IP:** IP that send the packet
    
- **Destination IP:** IP that is supposed to receive the packet
    
- **Sequence Number:** random number that functions as the ISN
    
- **Acknowledgment Number:** data that has is sequence number+1
    
- **Checksum:** calculation that will differ when a packet has been corrupted.
    
- **Data:** stores the data, i.e. bytes of a file
    
- **Flag:** determines how a packet is supposed to be handled.

## UDP/IP 

The *User Datagram Protocol (UDP)* is used for communication between devices when loss of data can be tolerated. It is a stateless protocol, which does not require a constant connection. For example no three-way handshake does not occur, neither does synchronization between the two devices. Because there is no process to set up a connection, there is no regard if the send data is received.

UDP packets are simpler than TCP packets and have fewer headers. There are however a few that both have in common:

- **Time to Live (TTL)**
    
- **Source Address**
    
- **Destination Address**
    
- **Source Port**
    
- **Destination Port**
    
- **Data**
      

## Ports 101

Ports are points in which data can be exchanged, any data will be sent or received through these ports. In computing these are values between 0 and 65535. Network devices use ports to enforce strict rules when communicating.

The ports from 0 and 1024 are known as **common ports**:

|  | Port Number | Use |
| :--: | :--: | :--: |
| File Transfer Protocol (FTP) | 21 | Used for file sharing applications from a client-server model |
| Secure Shell (SSH) | 22 | Used to securely login to systems via text interface |
| Hypertext Transfer Protocol (HTTP) | 80 | Used by the browser to download contents of web pages |
| Hypertext Transfer Protocol Secure (HTTPS) | 443 | Like HTTP bur with encryption |
| Server Message Block (SMB) | 445 | Similar to FTP but also allows to share devices like printers |
| Remote Desktop Protocol (RDP) | 3389 | Used to securely login to a system using a visual desktop interface |

## Extending Your Network

### Introduction to Port Forwarding

**Port forwarding** allows application and services to connect to the internet, without it these would only be available to devices in the same network.

A server with the (private) IP “192.168.1.10” runs a webserver on port 80, but its is only available to other devices in the same network (called intranet). If it is supposed to be accessible to the internet, the administrators have to implement port forwarding. With this other networks can access the server using the public IP of its network.

### Firewalls 101

A **firewall** is a device that determines what traffic is allowed to enter or exit a network. A firewall is configured by an administrator and there are a few factors which traffic is permitted or denied:

- Where does the traffic come from? (What network?)
    
- Where is the traffic going to? (For which specific network is the traffic?)
    
- What port is the traffic for?
    
- What protocol is the traffic using? (Are TCP, UDP allowed?)
    

Firewalls use packet inspection to determine the answers to these questions.

Firewalls can be hardware that can handle a magnitude of data, residential routers found in homes to software such as “Snort”. They can be categorized into 2 to 5 categories, with the 2 primary being:

- **Statefull:** determines the behavior of the device based upon the entire connection, rather than inspecting individual packets; consumes many resources and is dynamic
    
- **Stateless:** uses a static set of rules to determine if individual packets are acceptable; only as effective as the rules defined within them; consume fewer resources; effective when receiving large amounts of traffic
    

### VPN Basics

A **Virtual Private Network (VPN)** allows devices in different networks to communicate securely by creating a tunnel, a dedicated path between each other, creating a private network. With this devices can be part of different networks but can themselves be part of their own private network.

The benefits of a VPN:

- Networks in different geographical locations can be connected.
    
- Offers privacy
    
- Offers anonymity
    

Some VPN technologies:

|  |  |
| :--: | :--: |
| PPP | Used by PPTP; allows for authentication and encryption of data; works by using a private key and public certificate (like SSH); not capable of leaving a network on its own (non-routable) |
| Point-to-Point Tunneling Protocol (PPTP) | Allows the data from PPP to travel and leave a network; easy to set up and supported by most devices; weakly encrypted compared to alternatives |
| Internet Protocol Security (IPSec) | Encrypts data using the Internet Protocol (IP) framework; difficult to set up compared to alternatives; strong encryption and supported by many devices |
 
### LAN Networking Devices

#### What is a Router

Routing is the process of data traveling across networks. Routing involves creating a path between networks. Routers operate at Layer 3 of the OSI model, often featuring a interactive interface with which admins can configure rules such as port forwarding or firewalling.

Routers are useful when devices are connected by many paths finding the most optimal path. Routers are dedicated devices and do not perform the same functions as as switches.

When two networks are connected by many routers it can not be clear what is the most optimal path. Different protocols will decide what path to take, factors include:

- What path is the shortest?
    
- What path is the most reliable?
    
- Which path has the fastest medium (e.g. copper or fibre)?
    

#### What is a Switch?

A switch is dedicated to connecting multiple devices using Ethernet cables. They can operate at both layer 2 or layer 3 of the OSI model, layer 2 switches can not operate at layer 3. Layer 2 switches are solely responsible for forwarding frames onto the connected devices using their MAC address.

Layer 3 switches will send frames to the correct device like layer 2 switches but also route packets to the devices using the IP protocol.

Switches can also be used to create a _Virtual Local Area Network_ _(VLAN)_, allowing a network to be virtually split up. The split means that all devices can still access the internet but are treated separately.