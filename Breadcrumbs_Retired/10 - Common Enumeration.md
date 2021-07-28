# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ sudo nmap -p- --min-rate 10000 -oA nmap/breadcrumbs 10.10.10.228                                                                                [sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-28 16:06 PKT
Nmap scan report for 10.10.10.228
Host is up (0.11s latency).
Not shown: 65520 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
443/tcp   open  https
445/tcp   open  microsoft-ds
3306/tcp  open  mysql
5040/tcp  open  unknown
7680/tcp  open  pando-pub
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 11.64 seconds
```
Detected ports scan with service version and os detection.
```bash

```