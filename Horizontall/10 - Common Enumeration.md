# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
 ❯ sudo nmap -p- --min-rate 10000 -oA nmap/horizontall 10.10.11.105
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-09 19:22 PKT
Nmap scan report for 10.10.11.105
Host is up (0.11s latency).
Not shown: 65533 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 11.41 seconds
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p22,80 -A -oA nmap/horizontall_version 10.10.11.105
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-09 19:25 PKT
Nmap scan report for 10.10.11.105
Host is up (0.10s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 ee:77:41:43:d4:82:bd:3e:6e:6e:50:cd:ff:6b:0d:d5 (RSA)
|   256 3a:d5:89:d5:da:95:59:d9:df:01:68:37:ca:d5:10:b0 (ECDSA)
|_  256 4a:00:04:b4:9d:29:e7:af:37:16:1b:4f:80:2d:98:94 (ED25519)
80/tcp open  http    nginx 1.14.0 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: WAP|phone
Running: Linux 2.4.X|2.6.X, Sony Ericsson embedded
OS CPE: cpe:/o:linux:linux_kernel:2.4.20 cpe:/o:linux:linux_kernel:2.6.22 cpe:/h:sonyericsson:u8i_vivaz
OS details: Tomato 1.28 (Linux 2.4.20), Tomato firmware (Linux 2.6.22), Sony Ericsson U8i Vivaz mobile phone
Network Distance: 10 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT       ADDRESS
1   104.24 ms 10.10.14.1
2   ... 9
10  104.67 ms 10.10.11.105

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 118.96 seconds
```
## Adding horizontall.htb to /etc/hosts file
The ip address `10.10.11.105` corresponds to `horizontall.htb` domain which is not present in our hosts file.
![[Pasted image 20210909193152.png]]
Adding `horizontall.htb` to `/etc/hosts` file.
![[Pasted image 20210909193357.png]]
## Found another domain in 
