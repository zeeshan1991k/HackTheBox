# Common Enumeration
## NMAP
Nmap scan of all tcp ports.
```bash
sudo nmap -p- --min-rate 10000 -oA nmap/tenet 10.10.10.223
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-12 16:42 PKT
Warning: 10.10.10.223 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.10.223
Host is up (0.11s latency).
Not shown: 64541 closed ports, 992 filtered ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```
Nmap scan of detected ports with service and OS versions.
```bash
sudo nmap -p22,80 --min-rate 10000 -A -oA nmap/tenet_version 10.10.10.223
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-12 16:45 PKT
Stats: 0:00:01 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Parallel DNS resolution of 1 host. Timing: About 0.00% done
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 16:45 (0:00:00 remaining)
Stats: 0:00:07 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 16:45 (0:00:06 remaining)
Nmap scan report for 10.10.10.223
Host is up (0.11s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 cc:ca:43:d4:4c:e7:4e:bf:26:f4:27:ea:b8:75:a8:f8 (RSA)
|   256 85:f3:ac:ba:1a:6a:03:59:e2:7e:86:47:e7:3e:3c:00 (ECDSA)
|_  256 e7:e9:9a:dd:c3:4a:2f:7a:e1:e0:5d:a2:b0:ca:44:a8 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 - 5.4 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 22/tcp)
HOP RTT       ADDRESS
1   108.40 ms 10.10.14.1
2   108.78 ms 10.10.10.223

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.44 seconds
```
## Gobuster
gobuster for http://10.10.10.223
```bash
~/Dropbox/Documents/htb/boxes/tenet ❯ gobuster dir -u http://10.10.10.223 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  -o gobuster/http-root.log -t 100
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.223
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/04/12 12:19:29 Starting gobuster in directory enumeration mode
===============================================================
/wordpress            (Status: 301) [Size: 316] [--> http://10.10.10.223/wordpress/]
/server-status        (Status: 403) [Size: 277]

===============================================================
2021/04/12 12:24:27 Finished
===============================================================
```
gobuster for tenet.htb
```bash
~/Dropbox/Documents/htb/boxes/tenet 1m 46s ❯ gobuster dir -u http://tenet.htb/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  -o gobuster/http-tenet.log -t 100
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://tenet.htb/
[+] Method:                  GET
[+] Threads:                 100
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/04/12 12:51:39 Starting gobuster in directory enumeration mode
===============================================================
/wp-content           (Status: 301) [Size: 311] [--> http://tenet.htb/wp-content/]
/wp-includes          (Status: 301) [Size: 312] [--> http://tenet.htb/wp-includes/]
/wp-admin             (Status: 301) [Size: 309] [--> http://tenet.htb/wp-admin/]
/server-status        (Status: 403) [Size: 274]

===============================================================
2021/04/12 12:56:54 Finished
===============================================================
```
