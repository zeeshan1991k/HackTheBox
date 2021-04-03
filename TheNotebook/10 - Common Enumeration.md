# Common Enumeration
## NMAP
Nmap Scan of all ports. 
```bash
# Nmap 7.91 scan initiated Sat Apr  3 17:49:07 2021 as: nmap -p- --min-rate 10000 -oA nmap/TheNotebook 10.10.10.230
Nmap scan report for 10.10.10.230
Host is up (0.13s latency).
Not shown: 65532 closed ports
PORT      STATE    SERVICE
22/tcp    open     ssh
80/tcp    open     http
10010/tcp filtered rxapi
```

Nmap scan of found ports with Services and OS discovery 
```bash
# Nmap 7.91 scan initiated Sat Apr  3 17:53:32 2021 as: nmap -p22,80,10010 -A -oA nmap/TheNotebook_version 10.10.10.230
Nmap scan report for 10.10.10.230
Host is up (0.11s latency).

PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 86:df:10:fd:27:a3:fb:d8:36:a7:ed:90:95:33:f5:bf (RSA)
|   256 e7:81:d6:6c:df:ce:b7:30:03:91:5c:b5:13:42:06:44 (ECDSA)
|_  256 c6:06:34:c7:fc:00:c4:62:06:c2:36:0e:ee:5e:bf:6b (ED25519)
80/tcp    open     http    nginx 1.14.0 (Ubuntu)
10010/tcp filtered rxapi
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   103.13 ms 10.10.14.1
2   103.47 ms 10.10.10.230

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Apr  3 17:54:49 2021 -- 1 IP address (1 host up) scanned in 76.91 seconds
```
