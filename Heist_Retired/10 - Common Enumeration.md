# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/heist 10.10.10.149
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-11 00:01 PKT
Nmap scan report for 10.10.10.149
Host is up (0.18s latency).
Not shown: 65531 filtered ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
445/tcp  open  microsoft-ds
5985/tcp open  wsman

Nmap done: 1 IP address (1 host up) scanned in 14.85 seconds
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p80,135,445,5985 -A -oA nmap/heist_version 10.10.10.149
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-11 00:06 PKT
Nmap scan report for 10.10.10.149
Host is up (0.12s latency).

PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
| http-title: Support Login Page
|_Requested resource was login.php
135/tcp  open  msrpc         Microsoft Windows RPC
445/tcp  open  microsoft-ds?
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
OS fingerprint not ideal because: Missing a closed TCP port so results incomplete
No OS matches for host
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2021-07-10T19:06:32
|_  start_date: N/A

TRACEROUTE (using port 445/tcp)
HOP RTT       ADDRESS
1   112.89 ms 10.10.14.1
2   113.05 ms 10.10.10.149

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 60.96 seconds
```
Logging in as guest gives config.txt file.
![[Pasted image 20210711013543.png]]
This file contains hash `$1$pdQG$o8nrSzsGXeaduXrjlvKc91` which when cracked using hashcat gives `stealth1agent` using command `hashcat -m 500 hash.hash /content/drive/MyDrive/rockyou.txt`.
http://10.10.10.149/issues.php webpage has a user `Hazard` which can be used with cracked hash to access shares of user `Hazard`.
`smbmap` can be used to know the shares of `Hazard` user.
```bash
❯ smbmap -H 10.10.10.149 -u Hazard -p stealth1agent
[+] IP: 10.10.10.149:445        Name: 10.10.10.149
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
```
Found some more users via Impacker module lookupsid.py.
```bash
❯ python3 /home/kali/.local/bin/lookupsid.py SUPPORTDESK/Hazard:stealth1agent@10.10.10.149
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Brute forcing SIDs at 10.10.10.149
[*] StringBinding ncacn_np:10.10.10.149[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-4254423774-1266059056-3197185112
500: SUPPORTDESK\Administrator (SidTypeUser)
501: SUPPORTDESK\Guest (SidTypeUser)
503: SUPPORTDESK\DefaultAccount (SidTypeUser)
504: SUPPORTDESK\WDAGUtilityAccount (SidTypeUser)
513: SUPPORTDESK\None (SidTypeGroup)
1008: SUPPORTDESK\Hazard (SidTypeUser)
1009: SUPPORTDESK\support (SidTypeUser)
1012: SUPPORTDESK\Chase (SidTypeUser)
1013: SUPPORTDESK\Jason (SidTypeUser)
```



