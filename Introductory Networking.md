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