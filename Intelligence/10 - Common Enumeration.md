# Common Enumeration
##  NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/intelligence 10.10.10.248
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-19 19:39 PKT
Nmap scan report for 10.10.10.248
Host is up (0.11s latency).
Not shown: 65521 filtered ports
PORT      STATE SERVICE
53/tcp    open  domain
80/tcp    open  http
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
5985/tcp  open  wsman
49692/tcp open  unknown
49714/tcp open  unknown
50962/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 13.70 seconds
```
Detected ports scan with service version and os detection.
```bash

```