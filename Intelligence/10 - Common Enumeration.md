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
❯ sudo nmap -p53,80,88,135,139,389,445,593,636,3268,5985,49692,49714,50962 -sC  -sV -A -oN nmap/intelligence_all 10.10.10.248

Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-22 10:27 PKT
Nmap scan report for intelligence.htb (10.10.10.248)
Host is up (0.11s latency).

PORT      STATE    SERVICE       VERSION
53/tcp    open     domain        Simple DNS Plus
80/tcp    open     http          Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Intelligence
88/tcp    open     kerberos-sec  Microsoft Windows Kerberos (server time: 2021-09-22 12:27:52Z)
135/tcp   open     msrpc         Microsoft Windows RPC
139/tcp   open     netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open     ldap          Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername:<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
|_ssl-date: 2021-09-22T12:29:27+00:00; +7h00m02s from scanner time.
445/tcp   open     microsoft-ds?
593/tcp   open     ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open     ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername:<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
|_ssl-date: 2021-09-22T12:29:27+00:00; +7h00m02s from scanner time.
3268/tcp  open     ldap          Microsoft Windows Active Directory LDAP (Domain: intelligence.htb0., Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=dc.intelligence.htb
| Subject Alternative Name: othername:<unsupported>, DNS:dc.intelligence.htb
| Not valid before: 2021-04-19T00:43:16
|_Not valid after:  2022-04-19T00:43:16
|_ssl-date: 2021-09-22T12:29:27+00:00; +7h00m02s from scanner time.
5985/tcp  open     http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49692/tcp open     ncacn_http    Microsoft Windows RPC over HTTP 1.0
49714/tcp open     msrpc         Microsoft Windows RPC
50962/tcp filtered unknown
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
Network Distance: 2 hops
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 7h00m01s, deviation: 0s, median: 7h00m01s
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2021-09-22T12:28:47
|_  start_date: N/A

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   107.25 ms 10.10.14.1
2   107.39 ms intelligence.htb (10.10.10.248)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 104.39 seconds
```
## Added intelligence.htb to hosts file
![[Pasted image 20210919214945.png]]