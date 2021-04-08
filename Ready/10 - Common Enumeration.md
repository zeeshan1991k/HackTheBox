# Common Enumeration
## NMAP 
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/ready 10.10.10.220
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-08 15:17 PKT
Warning: 10.10.10.220 giving up on port because retransmission cap hit (10).
Nmap scan report for ready.htb (10.10.10.220)
Host is up (0.15s latency).
Not shown: 44037 closed ports, 21496 filtered ports
PORT     STATE SERVICE
22/tcp   open  ssh
5080/tcp open  onscreen
```
Detected ports' scan with service version and os detection.
```bash
❯ sudo nmap -p22,5080 --min-rate 10000 -A  -oA nmap/ready_version 10.10.10.220
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-08 15:25 PKT
Stats: 0:00:07 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 15:25 (0:00:06 remaining)
Nmap scan report for ready.htb (10.10.10.220)
Host is up (0.11s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 48:ad:d5:b8:3a:9f:bc:be:f7:e8:20:1e:f6:bf:de:ae (RSA)
|   256 b7:89:6c:0b:20:ed:49:b2:c1:86:7c:29:92:74:1c:1f (ECDSA)
|_  256 18:cd:9d:08:a6:21:a8:b8:b6:f7:9f:8d:40:51:54:fb (ED25519)
5080/tcp open  http    nginx
| http-robots.txt: 53 disallowed entries (15 shown)
| / /autocomplete/users /search /api /admin /profile
| /dashboard /projects/new /groups/new /groups/*/edit /users /help
|_/s/ /snippets/new /snippets/*/edit
| http-title: Sign in \xC2\xB7 GitLab
|_Requested resource was http://ready.htb:5080/users/sign_in
|_http-trane-info: Problem with XML parsing of /evox/about
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.4 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   107.45 ms 10.10.14.1
2   107.82 ms ready.htb (10.10.10.220)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.45 seconds
```
