# OSI and TCP/IP Model

> [!info]
>You should be familiar with the  [[Network Fundamentals#OSI Model|OSI]] and [[Network Fundamentals#TCP/IP Model|TCP/IP]] model
# Networking Tools

## Ping
The `ping` command is used when testing if a connection to a remote system is possible. The target can be a website or a computer. It uses the *Internet Control Message Protocol (ICMP)*, which works on the [[Network Fundamentals#Layer 3 – Network|Network layer]] of the OSI and on the Internet layer of the TCP/IP model.
The syntax for the command is `ping <target>`, you can either enter a website domain or an IP address:
```
ping google.com
> PING google.com (216.58.198.174) 
ping 216.58.198.174
```
When entering a website it also retruns its IP address. Nearly all networking devices, all OS and most embedded devices, can use `ping`.

## Traceroute
Traceroute lists all servers your request takes to get to the requested resource. On Windows `tracert destination` uses the ICMP protocol and on Linux `traceroute destination` uses UDP, but both can be changed with flags. 
## WHOIS
The `whois domain` command lets you query a domain name and get potentially a lot of information about who registered the website. There is also a [web version](https://www.whois.com/whois/).
Note that in Europe personal details are redacted.
## Dig
If unsure how [[How the Web works#DNS in detail|DNS]] works you should refresh your memory.
Dig lets you query a recursive DNS server of your choice for information abour a domain, and is useful for network troubleshooting.
The syntax is `dig domain@Dns-server-ip`, for example:
```
dig google.com@1.1.1.1
```

# Further reading
the  [CISCO Self Study Guide by Steve McQuerry](https://www.amazon.co.uk/Interconnecting-Cisco-Network-Devices-ICND1/dp/1587054620/ref=sr_1_1?keywords=Interconnecting+Cisco+Network+Devices%2C+Part+1&qid=1583683766&sr=8-1) is a ,albeit not very up to date, great and still relevant source of information about networking principles. 