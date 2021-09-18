# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/previse 10.10.11.104
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-16 17:50 PKT
Nmap scan report for 10.10.11.104
Host is up (0.11s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 11.36 seconds
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p22,80 -A -oA nmap/previse_version 10.10.11.104
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-16 17:52 PKT
Nmap scan report for 10.10.11.104
Host is up (0.11s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 53:ed:44:40:11:6e:8b:da:69:85:79:c0:81:f2:3a:12 (RSA)
|   256 bc:54:20:ac:17:23:bb:50:20:f4:e1:6e:62:0f:01:b5 (ECDSA)
|_  256 33:c1:89:ea:59:73:b1:78:84:38:a4:21:10:0c:91:d8 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-title: Previse Login
|_Requested resource was login.php
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT       ADDRESS
1   105.93 ms 10.10.14.1
2   106.49 ms 10.10.11.104

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.19 seconds
```
## Gobuster
Gobuster 
```bash
~/Dropbox/Documents/htb/boxes/previse ❯ cat gobuster/http-root_files.gobuster
/index.php            (Status: 302) [Size: 2801] [--> login.php]
/config.php           (Status: 200) [Size: 0]
/download.php         (Status: 302) [Size: 0] [--> login.php]
/login.php            (Status: 200) [Size: 2224]
/footer.php           (Status: 200) [Size: 217]
/header.php           (Status: 200) [Size: 980]
/favicon.ico          (Status: 200) [Size: 15406]
/.htaccess            (Status: 403) [Size: 277]
/logout.php           (Status: 302) [Size: 0] [--> login.php]
/.                    (Status: 302) [Size: 2801] [--> login.php]
/.html                (Status: 403) [Size: 277]
/.php                 (Status: 403) [Size: 277]
/status.php           (Status: 302) [Size: 2966] [--> login.php]
/.htpasswd            (Status: 403) [Size: 277]
/.htm                 (Status: 403) [Size: 277]
/.htpasswds           (Status: 403) [Size: 277]
/nav.php              (Status: 200) [Size: 1248]
/accounts.php         (Status: 302) [Size: 3994] [--> login.php]
/files.php            (Status: 302) [Size: 4914] [--> login.php]
/.htgroup             (Status: 403) [Size: 277]
/wp-forum.phps        (Status: 403) [Size: 277]
/.htaccess.bak        (Status: 403) [Size: 277]
/.htuser              (Status: 403) [Size: 277]
/.ht                  (Status: 403) [Size: 277]
/.htc                 (Status: 403) [Size: 277]
/.htaccess.old        (Status: 403) [Size: 277]
/.htacess             (Status: 403) [Size: 277]
```
