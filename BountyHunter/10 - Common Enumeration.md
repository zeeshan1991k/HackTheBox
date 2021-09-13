# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/bountyhunter 10.10.11.100
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-13 12:00 PKT
Nmap scan report for 10.10.11.100
Host is up (0.11s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 10.75 seconds
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p22,80 -A -oA nmap/bountyhunter_version 10.10.11.100
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-13 12:02 PKT
Nmap scan report for 10.10.11.100
Host is up (0.10s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 d4:4c:f5:79:9a:79:a3:b0:f1:66:25:52:c9:53:1f:e1 (RSA)
|   256 a2:1e:67:61:8d:2f:7a:37:a7:ba:3b:51:08:e8:89:a6 (ECDSA)
|_  256 a5:75:16:d9:69:58:50:4a:14:11:7a:42:c1:b6:23:44 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Bounty Hunters
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 80/tcp)
HOP RTT      ADDRESS
1   99.97 ms 10.10.14.1
2   99.60 ms 10.10.11.100

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.62 seconds
~/Dropbox/Documents/htb/boxes/bountyhunter 16s ❯
```
## Gobuster
```bash
 ❯ gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt -u  http://10.10.11.100 -o gobuster/http-root.gobuster
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.11.100
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/09/13 12:03:38 Starting gobuster in directory enumeration mode
===============================================================
/js                   (Status: 301) [Size: 309] [--> http://10.10.11.100/js/]
/css                  (Status: 301) [Size: 310] [--> http://10.10.11.100/css/]
/assets               (Status: 301) [Size: 313] [--> http://10.10.11.100/assets/]
/resources            (Status: 301) [Size: 316] [--> http://10.10.11.100/resources/]
/server-status        (Status: 403) [Size: 277]
Progress: 23994 / 62284 (38.52%)                                                   [ERROR] 2021/09/13 12:07:55 [!] parse "http://10.10.11.100/error\x1f_log": net/url: invalid control character in URL

===============================================================
2021/09/13 12:14:49 Finished
===============================================================
```
