# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ sudo nmap -p- --min-rate 10000 -oA nmap/breadcrumbs 10.10.10.228                                                                                [sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-28 16:06 PKT
Nmap scan report for 10.10.10.228
Host is up (0.11s latency).
Not shown: 65520 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
443/tcp   open  https
445/tcp   open  microsoft-ds
3306/tcp  open  mysql
5040/tcp  open  unknown
7680/tcp  open  pando-pub
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 11.64 seconds
```
Detected ports scan with service version and os detection.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired 4m 26s ❯ sudo nmap -p22,80,135,139,443,445,3306,5040,7680,49664,49665,49666,49667,49668,49669 -A  -oA nmap/breadcrumbs_version 10.10.10.228
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-28 16:15 PKT
Nmap scan report for 10.10.10.228
Host is up (0.11s latency).

PORT      STATE SERVICE       VERSION
22/tcp    open  ssh           OpenSSH for_Windows_7.7 (protocol 2.0)
| ssh-hostkey:
|   2048 9d:d0:b8:81:55:54:ea:0f:89:b1:10:32:33:6a:a7:8f (RSA)
|   256 1f:2e:67:37:1a:b8:91:1d:5c:31:59:c7:c6:df:14:1d (ECDSA)
|_  256 30:9e:5d:12:e3:c6:b7:c6:3b:7e:1e:e7:89:7e:83:e4 (ED25519)
80/tcp    open  http          Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1h PHP/8.0.1)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
443/tcp   open  ssl/http      Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1h PHP/8.0.1)
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
| tls-alpn:
|_  http/1.1
445/tcp   open  microsoft-ds?
3306/tcp  open  mysql?
| fingerprint-strings:
|   NULL, giop:
|_    Host '10.10.14.105' is not allowed to connect to this MariaDB server
5040/tcp  open  unknown
7680/tcp  open  pando-pub?
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49669/tcp open  unknown
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3306-TCP:V=7.91%I=7%D=7/28%Time=61013C70%P=x86_64-pc-linux-gnu%r(NU
SF:LL,4B,"G\0\0\x01\xffj\x04Host\x20'10\.10\.14\.105'\x20is\x20not\x20allo
SF:wed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(giop,4B,"G\
SF:0\0\x01\xffj\x04Host\x20'10\.10\.14\.105'\x20is\x20not\x20allowed\x20to
SF:\x20connect\x20to\x20this\x20MariaDB\x20server");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows XP (86%)
OS CPE: cpe:/o:microsoft:windows_xp::sp2
Aggressive OS guesses: Microsoft Windows XP SP2 (86%), Microsoft Windows XP SP3 (86%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   2.02:
|_    Message signing enabled but not required
|_smb2-time: Protocol negotiation failed (SMB2)

TRACEROUTE (using port 445/tcp)
HOP RTT       ADDRESS
1   111.52 ms 10.10.14.1
2   112.16 ms 10.10.10.228

OS and Service detection performed. Please report any incorrect results
```
## SMB	
Did not find anything.
## SQL Injection
Checked for SQL Injection via `a',a'-- -,a"-- -,a-- -,then a`on http://10.10.10.228/php/books.php  in `title` and `author` field but found nothing interesting.
## Gobuster
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-small-words-lowercase.txt -x php -u http://10.10.10.228 -o root.gobuster -t 100
/.html                (Status: 403) [Size: 301]
/js                   (Status: 301) [Size: 333] [--> http://10.10.10.228/js/]
/includes             (Status: 301) [Size: 339] [--> http://10.10.10.228/includes/]
/css                  (Status: 301) [Size: 334] [--> http://10.10.10.228/css/]
/.htm                 (Status: 403) [Size: 301]
/.html.php            (Status: 403) [Size: 301]
/index.php            (Status: 200) [Size: 2368]
/.htm.php             (Status: 403) [Size: 301]
/db                   (Status: 301) [Size: 333] [--> http://10.10.10.228/db/]
/php                  (Status: 301) [Size: 334] [--> http://10.10.10.228/php/]
/webalizer            (Status: 403) [Size: 301]
/.                    (Status: 200) [Size: 2368]
/portal               (Status: 301) [Size: 337] [--> http://10.10.10.228/portal/]
/phpmyadmin           (Status: 403) [Size: 301]
/.htaccess            (Status: 403) [Size: 301]
/.htaccess.php        (Status: 403) [Size: 301]
/books                (Status: 301) [Size: 336] [--> http://10.10.10.228/books/]
/examples             (Status: 503) [Size: 401]
/.htc                 (Status: 403) [Size: 301]
/.htc.php             (Status: 403) [Size: 301]
/.html_var_de         (Status: 403) [Size: 301]
/.html_var_de.php     (Status: 403) [Size: 301]
/licenses             (Status: 403) [Size: 420]
/server-status        (Status: 403) [Size: 420]
/.htpasswd            (Status: 403) [Size: 301]
/.htpasswd.php        (Status: 403) [Size: 301]
/con.php              (Status: 403) [Size: 301]
/con                  (Status: 403) [Size: 301]
/.html.               (Status: 403) [Size: 301]
/.html..php           (Status: 403) [Size: 301]
/.html.html           (Status: 403) [Size: 301]
/.html.html.php       (Status: 403) [Size: 301]
/.htpasswds           (Status: 403) [Size: 301]
/.htpasswds.php       (Status: 403) [Size: 301]
/.htm.                (Status: 403) [Size: 301]
/.htm..php            (Status: 403) [Size: 301]
/.htmll               (Status: 403) [Size: 301]
/.htmll.php           (Status: 403) [Size: 301]
```


