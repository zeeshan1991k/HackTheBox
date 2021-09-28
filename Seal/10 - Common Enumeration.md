# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
 ❯ sudo nmap -p- --min-rate 10000 -oA nmap/seal 10.10.10.250
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-22 17:27 PKT
Warning: 10.10.10.250 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.10.250
Host is up (0.10s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
443/tcp  open  https
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 13.49 seconds
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p22,443,8080 -sC  -sV -A -oA nmap/seal_all 10.10.10.250
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-22 17:30 PKT
Nmap scan report for 10.10.10.250
Host is up (0.10s latency).

PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 4b:89:47:39:67:3d:07:31:5e:3f:4c:27:41:1f:f9:67 (RSA)
|   256 04:a7:4f:39:95:65:c5:b0:8d:d5:49:2e:d8:44:00:36 (ECDSA)
|_  256 b4:5e:83:93:c5:42:49:de:71:25:92:71:23:b1:85:54 (ED25519)
443/tcp  open  ssl/http   nginx 1.18.0 (Ubuntu)
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Seal Market
| ssl-cert: Subject: commonName=seal.htb/organizationName=Seal Pvt Ltd/stateOrProvinceName=London/countryName=UK
| Not valid before: 2021-05-05T10:24:03
|_Not valid after:  2022-05-05T10:24:03
| tls-alpn:
|_  http/1.1
| tls-nextprotoneg:
|_  http/1.1
8080/tcp open  http-proxy
| fingerprint-strings:
|   FourOhFourRequest:
|     HTTP/1.1 401 Unauthorized
|     Date: Wed, 22 Sep 2021 12:31:03 GMT
|     Set-Cookie: JSESSIONID=node01m2nr3x07x5jn1ooaqiqg3k3t040.node0; Path=/; HttpOnly
|     Expires: Thu, 01 Jan 1970 00:00:00 GMT
|     Content-Type: text/html;charset=utf-8
|     Content-Length: 0
|   GetRequest:
|     HTTP/1.1 401 Unauthorized
|     Date: Wed, 22 Sep 2021 12:31:02 GMT
|     Set-Cookie: JSESSIONID=node0mki19i8y2l5l1hsvv9wvcdnk638.node0; Path=/; HttpOnly
|     Expires: Thu, 01 Jan 1970 00:00:00 GMT
|     Content-Type: text/html;charset=utf-8
|     Content-Length: 0
|   HTTPOptions:
|     HTTP/1.1 200 OK
|     Date: Wed, 22 Sep 2021 12:31:03 GMT
|     Set-Cookie: JSESSIONID=node019l4qtgmaduuvgeuo9u5zpqn339.node0; Path=/; HttpOnly
|     Expires: Thu, 01 Jan 1970 00:00:00 GMT
|     Content-Type: text/html;charset=utf-8
|     Allow: GET,HEAD,POST,OPTIONS
|     Content-Length: 0
|   RPCCheck:
|     HTTP/1.1 400 Illegal character OTEXT=0x80
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 71
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: Illegal character OTEXT=0x80</pre>
|   RTSPRequest:
|     HTTP/1.1 505 Unknown Version
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 58
|     Connection: close
|     <h1>Bad Message 505</h1><pre>reason: Unknown Version</pre>
|   Socks4:
|     HTTP/1.1 400 Illegal character CNTL=0x4
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 69
|     Connection: close
|     <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x4</pre>
|   Socks5:
|     HTTP/1.1 400 Illegal character CNTL=0x5
|     Content-Type: text/html;charset=iso-8859-1
|     Content-Length: 69
|     Connection: close
|_    <h1>Bad Message 400</h1><pre>reason: Illegal character CNTL=0x5</pre>
| http-auth:
| HTTP/1.1 401 Unauthorized\x0D
|_  Server returned status 401 but no WWW-Authenticate header.
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.91%I=7%D=9/22%Time=614B2206%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,F5,"HTTP/1\.1\x20401\x20Unauthorized\r\nDate:\x20Wed,\x2022\x2
SF:0Sep\x202021\x2012:31:02\x20GMT\r\nSet-Cookie:\x20JSESSIONID=node0mki19
SF:i8y2l5l1hsvv9wvcdnk638\.node0;\x20Path=/;\x20HttpOnly\r\nExpires:\x20Th
SF:u,\x2001\x20Jan\x201970\x2000:00:00\x20GMT\r\nContent-Type:\x20text/htm
SF:l;charset=utf-8\r\nContent-Length:\x200\r\n\r\n")%r(HTTPOptions,109,"HT
SF:TP/1\.1\x20200\x20OK\r\nDate:\x20Wed,\x2022\x20Sep\x202021\x2012:31:03\
SF:x20GMT\r\nSet-Cookie:\x20JSESSIONID=node019l4qtgmaduuvgeuo9u5zpqn339\.n
SF:ode0;\x20Path=/;\x20HttpOnly\r\nExpires:\x20Thu,\x2001\x20Jan\x201970\x
SF:2000:00:00\x20GMT\r\nContent-Type:\x20text/html;charset=utf-8\r\nAllow:
SF:\x20GET,HEAD,POST,OPTIONS\r\nContent-Length:\x200\r\n\r\n")%r(RTSPReque
SF:st,AD,"HTTP/1\.1\x20505\x20Unknown\x20Version\r\nContent-Type:\x20text/
SF:html;charset=iso-8859-1\r\nContent-Length:\x2058\r\nConnection:\x20clos
SF:e\r\n\r\n<h1>Bad\x20Message\x20505</h1><pre>reason:\x20Unknown\x20Versi
SF:on</pre>")%r(FourOhFourRequest,F6,"HTTP/1\.1\x20401\x20Unauthorized\r\n
SF:Date:\x20Wed,\x2022\x20Sep\x202021\x2012:31:03\x20GMT\r\nSet-Cookie:\x2
SF:0JSESSIONID=node01m2nr3x07x5jn1ooaqiqg3k3t040\.node0;\x20Path=/;\x20Htt
SF:pOnly\r\nExpires:\x20Thu,\x2001\x20Jan\x201970\x2000:00:00\x20GMT\r\nCo
SF:ntent-Type:\x20text/html;charset=utf-8\r\nContent-Length:\x200\r\n\r\n"
SF:)%r(Socks5,C3,"HTTP/1\.1\x20400\x20Illegal\x20character\x20CNTL=0x5\r\n
SF:Content-Type:\x20text/html;charset=iso-8859-1\r\nContent-Length:\x2069\
SF:r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Message\x20400</h1><pre>reaso
SF:n:\x20Illegal\x20character\x20CNTL=0x5</pre>")%r(Socks4,C3,"HTTP/1\.1\x
SF:20400\x20Illegal\x20character\x20CNTL=0x4\r\nContent-Type:\x20text/html
SF:;charset=iso-8859-1\r\nContent-Length:\x2069\r\nConnection:\x20close\r\
SF:n\r\n<h1>Bad\x20Message\x20400</h1><pre>reason:\x20Illegal\x20character
SF:\x20CNTL=0x4</pre>")%r(RPCCheck,C7,"HTTP/1\.1\x20400\x20Illegal\x20char
SF:acter\x20OTEXT=0x80\r\nContent-Type:\x20text/html;charset=iso-8859-1\r\
SF:nContent-Length:\x2071\r\nConnection:\x20close\r\n\r\n<h1>Bad\x20Messag
SF:e\x20400</h1><pre>reason:\x20Illegal\x20character\x20OTEXT=0x80</pre>");
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 2.6.32 (95%), Linux 5.0 - 5.3 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 5.0 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 443/tcp)
HOP RTT       ADDRESS
1   100.94 ms 10.10.14.1
2   100.67 ms 10.10.10.250

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.63 seconds
```
## Added seal.htb to /etc/hosts file
![[Pasted image 20210922182710.png]]
## Gobuster
