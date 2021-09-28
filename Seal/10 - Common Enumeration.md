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
Directory bruteforcing for http://10.10.10.250/.
```bash
❯ cat gobuster/http-root_raft.gobuster
/js                   (Status: 302) [Size: 0] [--> http://10.10.10.250/js/]
/admin                (Status: 302) [Size: 0] [--> http://10.10.10.250/admin/]
/images               (Status: 302) [Size: 0] [--> http://10.10.10.250/images/]
/css                  (Status: 302) [Size: 0] [--> http://10.10.10.250/css/]
/manager              (Status: 302) [Size: 0] [--> http://10.10.10.250/manager/]
/icon                 (Status: 302) [Size: 0] [--> http://10.10.10.250/icon/]
/develop              (Status: 302) [Size: 0] [--> http://10.10.10.250/develop/]
/[                    (Status: 400) [Size: 771]
/plain]               (Status: 400) [Size: 771]
/]                    (Status: 400) [Size: 771]
/quote]               (Status: 400) [Size: 771]
/extension]           (Status: 400) [Size: 771]
/[0-9]                (Status: 400) [Size: 771]
/[0-1][0-9]           (Status: 400) [Size: 771]
/20[0-9][0-9]         (Status: 400) [Size: 771]
/[2]                  (Status: 400) [Size: 771]
/host-manager         (Status: 302) [Size: 0] [--> http://10.10.10.250/host-manager/]
/[2-9]                (Status: 400) [Size: 771]
/options[]            (Status: 400) [Size: 771]
```
Directory bruteforcing for http://10.10.10.250/manger
```bash
❯ cat gobuster/http-manager_raft.gobuster
/images               (Status: 302) [Size: 0] [--> http://seal.htb/manager/images/]
/html                 (Status: 403) [Size: 162]
/htmlarea             (Status: 403) [Size: 162]
/text                 (Status: 401) [Size: 2499]
/status               (Status: 401) [Size: 2499]
/htmleditor           (Status: 403) [Size: 162]
/htmls                (Status: 403) [Size: 162]
/htmlemail            (Status: 403) [Size: 162]
/html2pdf             (Status: 403) [Size: 162]
/html_email           (Status: 403) [Size: 162]
/html2                (Status: 403) [Size: 162]
/[                    (Status: 400) [Size: 771]
/plain]               (Status: 400) [Size: 771]
/htmlimages           (Status: 403) [Size: 162]
/htmlMimeMail5        (Status: 403) [Size: 162]
/htmledit             (Status: 403) [Size: 162]
/htmlrotate           (Status: 403) [Size: 162]
/]                    (Status: 400) [Size: 771]
/html_editor          (Status: 403) [Size: 162]
/html_files           (Status: 403) [Size: 162]
/html_snippets        (Status: 403) [Size: 162]
/htmlpdf              (Status: 403) [Size: 162]
/html_templates       (Status: 403) [Size: 162]
/htmlfiles            (Status: 403) [Size: 162]
/htmltag              (Status: 403) [Size: 162]
/html5                (Status: 403) [Size: 162]
/html_emails          (Status: 403) [Size: 162]
/html_old             (Status: 403) [Size: 162]
/html_pages           (Status: 403) [Size: 162]
/htmlmail             (Status: 403) [Size: 162]
/quote]               (Status: 400) [Size: 771]
/extension]           (Status: 400) [Size: 771]
/html8                (Status: 403) [Size: 162]
/html-email           (Status: 403) [Size: 162]
/html-emails          (Status: 403) [Size: 162]
/html-kit             (Status: 403) [Size: 162]
/html2fpdf            (Status: 403) [Size: 162]
/html2ps              (Status: 403) [Size: 162]
/htmlEditor           (Status: 403) [Size: 162]
/htmlMimeMail         (Status: 403) [Size: 162]
/html_create          (Status: 403) [Size: 162]
/html_includes        (Status: 403) [Size: 162]
/html_mail            (Status: 403) [Size: 162]
/html_site            (Status: 403) [Size: 162]
/htmlemails           (Status: 403) [Size: 162]
/htmlnews             (Status: 403) [Size: 162]
/htmltemplates        (Status: 403) [Size: 162]
/[0-9]                (Status: 400) [Size: 771]
/20[0-9][0-9]         (Status: 400) [Size: 771]
/[0-1][0-9]           (Status: 400) [Size: 771]
/[2]                  (Status: 400) [Size: 771]
/html_wrap            (Status: 403) [Size: 162]
/html0                (Status: 403) [Size: 162]
/html1                (Status: 403) [Size: 162]
/htmlArea             (Status: 403) [Size: 162]
/htmlInclude          (Status: 403) [Size: 162]
/html_1               (Status: 403) [Size: 162]
/html_bbs             (Status: 403) [Size: 162]
/html_c               (Status: 403) [Size: 162]
/html_errors          (Status: 403) [Size: 162]
/html_file            (Status: 403) [Size: 162]
/html_images          (Status: 403) [Size: 162]
/html_mime            (Status: 403) [Size: 162]
/html_test_mail       (Status: 403) [Size: 162]
/html_tpl             (Status: 403) [Size: 162]
/htmlarea2            (Status: 403) [Size: 162]
/htmlarea4            (Status: 403) [Size: 162]
/htmlarea_full        (Status: 403) [Size: 162]
/htmlbackup           (Status: 403) [Size: 162]
/htmlblocks           (Status: 403) [Size: 162]
/htmlcache            (Status: 403) [Size: 162]
/htmldocs             (Status: 403) [Size: 162]
/htmle                (Status: 403) [Size: 162]
/htmlfile             (Status: 403) [Size: 162]
/htmlen               (Status: 403) [Size: 162]
/htmlguide            (Status: 403) [Size: 162]
/htmlold              (Status: 403) [Size: 162]
/htmlpages            (Status: 403) [Size: 162]
/htmlpurifier         (Status: 403) [Size: 162]
/htmlresp             (Status: 403) [Size: 162]
/htmlsite             (Status: 403) [Size: 162]
/htmlsource           (Status: 403) [Size: 162]
/htmltest             (Status: 403) [Size: 162]
/htmlets              (Status: 403) [Size: 162]
/[2-9]                (Status: 400) [Size: 771]
/options[]            (Status: 400) [Size: 771]
```
