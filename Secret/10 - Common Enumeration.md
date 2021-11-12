# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- -sU -sS  --min-rate 10000 -oA nmap/secret 10.10.11.120
Starting Nmap 7.92 ( https://nmap.org ) at 2021-11-12 13:12 PKT
Warning: 10.10.11.120 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.11.120
Host is up (0.11s latency).
Not shown: 65532 closed tcp ports (reset), 78 closed udp ports (port-unreach), 65457 open|filtered udp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3000/tcp open  ppp
```
Detected ports scan with service version and os detection.
```bash
 ❯ sudo nmap -sU -sT  -p22,80,3000 -sC -sV -A -oA nmap/secret_all 10.10.11.120
Starting Nmap 7.92 ( https://nmap.org ) at 2021-11-12 13:15 PKT
Nmap scan report for 10.10.11.120
Host is up (0.11s latency).

PORT     STATE  SERVICE VERSION
22/tcp   open   ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 97:af:61:44:10:89:b9:53:f0:80:3f:d7:19:b1:e2:9c (RSA)
|   256 95:ed:65:8d:cd:08:2b:55:dd:17:51:31:1e:3e:18:12 (ECDSA)
|_  256 33:7b:c1:71:d3:33:0f:92:4e:83:5a:1f:52:02:93:5e (ED25519)
80/tcp   open   http    nginx 1.18.0 (Ubuntu)
|_http-title: DUMB Docs
|_http-server-header: nginx/1.18.0 (Ubuntu)
3000/tcp open   http    Node.js (Express middleware)
|_http-title: DUMB Docs
22/udp   closed ssh
80/udp   closed http
3000/udp closed hbci
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using proto 1/icmp)
HOP RTT       ADDRESS
1   110.69 ms 10.10.14.1
2   110.47 ms 10.10.11.120

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.96 seconds
```
## Gobuster
```bash
 ❯ gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt  -u http://10.10.11.120 -o gobuster/http-root_raft.gobuster
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.11.120
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/raft-large-directories.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/11/12 13:31:41 Starting gobuster in directory enumeration mode
===============================================================
/download             (Status: 301) [Size: 183] [--> /download/]
/docs                 (Status: 200) [Size: 20720]
/api                  (Status: 200) [Size: 93]
/assets               (Status: 301) [Size: 179] [--> /assets/]
/API                  (Status: 200) [Size: 93]
/Docs                 (Status: 200) [Size: 20720]
/Api                  (Status: 200) [Size: 93]
/DOCS                 (Status: 200) [Size: 20720]
Progress: 24000 / 62284 (38.53%)                               [ERROR] 2021/11/12 13:36:28 [!] parse "http://10.10.11.120/error\x1f_log": net/url: invalid control character in URL

===============================================================
2021/11/12 13:43:47 Finished
===============================================================
```
