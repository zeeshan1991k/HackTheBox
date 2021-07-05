# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
sudo nmap -p- --min-rate 10000 -oA nmap/shocker 10.10.10.56                                                                                           
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-04 16:04 PKT
Nmap scan report for 10.10.10.56
Host is up (0.12s latency).
Not shown: 65533 closed ports
PORT     STATE SERVICE
80/tcp   open  http
2222/tcp open  EtherNetIP-1

Nmap done: 1 IP address (1 host up) scanned in 9.64 seconds
```
Detected ports scan with service version and os detection.
```bash
 sudo nmap -p80,2222 -A -oA nmap/shocker_version 10.10.10.56                                                                                          
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-04 16:06 PKT
Stats: 0:00:01 elapsed; 0 hosts completed (0 up), 0 undergoing Script Pre-Scan
NSE Timing: About 0.00% done
Stats: 0:00:07 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 16:06 (0:00:07 remaining)
Nmap scan report for 10.10.10.56
Host is up (0.12s latency).

PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 c4:f8:ad:e8:f8:04:77:de:cf:15:0d:63:0a:18:7e:49 (RSA)
|   256 22:8f:b1:97:bf:0f:17:08:fc:7e:2c:8f:e9:77:3a:48 (ECDSA)
|_  256 e6:ac:27:a3:b5:a9:f1:12:3c:34:a5:5d:5b:eb:3d:e9 (ED25519)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.12 (95%), Linux 3.13 (95%), Linux 3.16 (95%), Linux 3.18 (95%), Linux 3.2 - 4.9 (95%), Linux 3.8 - 3.11 (95%), Linux 4.4 (95%), Linux 4.2 (95%), Linux 4.8 (95%), ASUS RT-N56U WAP (Linux 3.4) (95%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 2222/tcp)
HOP RTT       ADDRESS
1   107.11 ms 10.10.14.1
2   107.27 ms 10.10.10.56

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.01 seconds
```

## Gobuster
```bash
gobuster dir -u http://10.10.10.56 -w /usr/share/seclists/Discovery/Web-Content/raft-small-files.txt -o ffuf/http-root.out -t 100    
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.56
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/raft-small-files.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/07/04 17:02:40 Starting gobuster in directory enumeration mode
===============================================================
/index.html           (Status: 200) [Size: 137]
/.htaccess            (Status: 403) [Size: 295]
/.                    (Status: 200) [Size: 137]
/.html                (Status: 403) [Size: 291]
/.htpasswd            (Status: 403) [Size: 295]
/.htm                 (Status: 403) [Size: 290]
/.htpasswds           (Status: 403) [Size: 296]
/.htgroup             (Status: 403) [Size: 294]
/.htaccess.bak        (Status: 403) [Size: 299]
/.htuser              (Status: 403) [Size: 293]

===============================================================
2021/07/04 17:02:54 Finished
===============================================================
```
Found user.sh
```bash
 gobuster dir -u http://10.10.10.56/cgi-bin/ -w /usr/share/wordlists/dirb/big.txt -x cgi,js,txt,php,sh,pl -o gobuster/http-big.out -t 100            
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.56/cgi-bin/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirb/big.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Extensions:              txt,php,sh,pl,cgi,js
[+] Timeout:                 10s
===============================================================
2021/07/05 11:15:18 Starting gobuster in directory enumeration mode
===============================================================
/.htaccess.pl         (Status: 403) [Size: 306]
/.htpasswd.pl         (Status: 403) [Size: 306]
/.htpasswd.cgi        (Status: 403) [Size: 307]
/.htaccess.cgi        (Status: 403) [Size: 307]
/.htpasswd            (Status: 403) [Size: 303]
/.htaccess.js         (Status: 403) [Size: 306]
/.htpasswd.js         (Status: 403) [Size: 306]
/.htaccess.txt        (Status: 403) [Size: 307]
/.htpasswd.txt        (Status: 403) [Size: 307]
/.htaccess.php        (Status: 403) [Size: 307]
/.htpasswd.php        (Status: 403) [Size: 307]
/.htaccess.sh         (Status: 403) [Size: 306]
/.htpasswd.sh         (Status: 403) [Size: 306]
/.htaccess            (Status: 403) [Size: 303]
/user.sh              (Status: 200) [Size: 118]
Progress: 134036 / 143290 (93.54%)            [ERROR] 2021/07/05 11:18:03 [!] Get "http://10.10.10.56/cgi-bin/translations.cgi": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
Progress: 143283 / 143290 (100.00%)           [ERROR] 2021/07/05 11:18:26 [!] Get "http://10.10.10.56/cgi-bin/xmllogs": context deadline exceeded (Client.Timeout exceeded while awaiting headers)

===============================================================
2021/07/05 11:18:27 Finished
===============================================================
```
