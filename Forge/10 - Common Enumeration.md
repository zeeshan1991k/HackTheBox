# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/forge 10.10.11.111
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-28 12:51 PKT
Nmap scan report for 10.10.11.111
Host is up (0.13s latency).
Not shown: 65532 closed ports
PORT   STATE    SERVICE
21/tcp filtered ftp
22/tcp open     ssh
80/tcp open     http

Nmap done: 1 IP address (1 host up) scanned in 12.06 seconds
```
All ports scan with service version and os detection.
```bash
❯ sudo nmap -p21,22,80 -sC  -sV -A -oA nmap/forge_all 10.10.11.111
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-28 12:54 PKT
Nmap scan report for 10.10.11.111
Host is up (0.10s latency).

PORT   STATE    SERVICE VERSION
21/tcp filtered ftp
22/tcp open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 4f:78:65:66:29:e4:87:6b:3c:cc:b4:3a:d2:57:20:ac (RSA)
|   256 79:df:3a:f1:fe:87:4a:57:b0:fd:4e:d0:54:c6:28:d9 (ECDSA)
|_  256 b0:58:11:40:6d:8c:bd:c5:72:aa:83:08:c5:51:fb:33 (ED25519)
80/tcp open     http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Did not follow redirect to http://forge.htb
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Linux 5.0 - 5.3 (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 - 5.4 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   110.50 ms 10.10.14.1
2   110.41 ms 10.10.11.111

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.48 seconds
```
## Added forge.htb to /etc/hosts file
![[Pasted image 20210928125657.png]]
## Finding another subdomain/vhost using wfuzz
```bash
❯ wfuzz -c -f gobuster/vhosts_forge_htb.gobuster -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt   -u "http://forge.htb" -H "Host: FUZZ.forge.htb" -t 42 |
 grep -v "302"
 ********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://forge.htb/
Total requests: 4989

=====================================================================
ID           Response   Lines    Word       Chars       Payload
=====================================================================

000000024:   200        1 L      4 W        27 Ch       "admin"

Total time: 0
Processed Requests: 4989
Filtered Requests: 0
Requests/sec.: 0
```
Found another subdomain `http://admin.forge.htb`  using wfuzz
## Added admin.forge.htb to /etc/hosts file
![[Pasted image 20210929181846.png]]
