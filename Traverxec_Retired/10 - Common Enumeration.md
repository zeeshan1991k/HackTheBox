# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/traverxec 10.10.10.165
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-09 13:09 PKT
Nmap scan report for 10.10.10.165
Host is up (0.16s latency).
Not shown: 65533 filtered ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 14.52 seconds
```
Detected ports scan with service version and os detection.
```bash

