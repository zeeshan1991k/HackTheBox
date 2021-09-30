# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- -sU -sS  --min-rate 10000 -oA nmap/writer 10.10.11.101
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-30 12:13 PKT
Warning: 10.10.11.101 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.101
Host is up (0.11s latency).
Not shown: 65610 closed ports, 65456 open|filtered ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 86.99 seconds
```
All ports scan with service version and os detection.
```bash

```