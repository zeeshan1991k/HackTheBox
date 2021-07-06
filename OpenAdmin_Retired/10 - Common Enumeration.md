# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
sudo nmap -p- --min-rate 10000 -oA nmap/openadmin 10.10.10.171
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-05 23:56 PKT
Nmap scan report for 10.10.10.171
Host is up (0.11s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 12.34 seconds
```
Detected ports scan with service version and os detection.
```bash
sudo nmap -p22,80 -A -oA nmap/openadmin_version 10.10.10.171
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-05 23:58 PKT
Nmap scan report for 10.10.10.171
Host is up (0.11s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 4b:98:df:85:d1:7e:f0:3d:da:48:cd:bc:92:00:b7:54 (RSA)
|   256 dc:eb:3d:c9:44:d1:18:b1:22:b4:cf:de:bd:6c:7a:54 (ECDSA)
|_  256 dc:ad:ca:3c:11:31:5b:6f:e6:a4:89:34:7c:9b:e5:50 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.2 - 4.9 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Linux 3.16 (93%), Linux 3.18 (93%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 5.1 (93%), Oracle VM Server 3.4.2 (Linux 4.1) (93%), Android 4.1.1 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   102.62 ms 10.10.14.1
2   102.87 ms 10.10.10.171

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.33 seconds
```
## Gobuster
```bash
❯ gobuster dir -u http://10.10.10.171/ -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt  -o gobuster/http-root.out -t 100

/music                (Status: 301) [Size: 312] [--> http://10.10.10.171/music/]
/artwork              (Status: 301) [Size: 314] [--> http://10.10.10.171/artwork/]
/sierra               (Status: 301) [Size: 313] [--> http://10.10.10.171/sierra/]
/server-status        (Status: 403) [Size: 277]

```
## Found another subdomain
Found another subdomain openadmin.htb
![[Pasted image 20210706005845.png]]
## Added new subdomain to hosts file
![[Pasted image 20210706010123.png]]