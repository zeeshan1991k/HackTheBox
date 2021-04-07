# Common Enumeration

## Nmap

* FTP over SSL
* HTTP Default Page
* Host: Debian 10+deb10u2
```bash
Starting Nmap 7.91 ( https://nmap.org ) at 2021-04-01 09:27 PKT
Nmap scan report for 10.10.10.208
Host is up (0.11s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
| ssl-cert: Subject: commonName=*.crossfit.htb/organizationName=Cross Fit Ltd./stateOrProvinceName=NY/countryName=US
| Not valid before: 2020-04-30T19:16:46
|_Not valid after:  3991-08-16T19:16:46
|_ssl-date: TLS randomness does not represent time
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey:
|   2048 b0:e7:5f:5f:7e:5a:4f:e8:e4:cf:f1:98:01:cb:3f:52 (RSA)
|   256 67:88:2d:20:a5:c1:a7:71:50:2b:c8:07:a4:b2:60:e5 (ECDSA)
|_  256 62:ce:a3:15:93:c8:8c:b6:8e:23:1d:66:52:f4:4f:ef (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Apache2 Debian Default Page: It works
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 4.15 - 5.6 (95%), Linux 5.3 - 5.4 (95%), Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (94%), Linux 2.6.32 (94%), Linux 5.0 - 5.3 (94%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (92%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: Cross; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 21/tcp)
HOP RTT       ADDRESS
1   111.49 ms 10.10.14.1
2   111.94 ms 10.10.10.208

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.52 seconds
```

## Grab SSL Banner FTP
```bash
❯ openssl s_client -connect 10.10.10.208:21 -starttls ftp
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 C = US, ST = NY, O = Cross Fit Ltd., CN = *.crossfit.htb, emailAddress = info@gym-club.crossfit.htb
verify error:num=18:self signed certificate
verify return:1
depth=0 C = US, ST = NY, O = Cross Fit Ltd., CN = *.crossfit.htb, emailAddress = info@gym-club.crossfit.htb
verify return:1
---
...[snip]...
```

## Gobuster
Gobuster did not return anything
```bash
❯ gobuster vhost -u http://crossfit.htb  -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -o gobuster/vhost.out -t 100
```

## Sub-Domain Bruteforce via CORS
Upon an HTTP `Request Header` having a valid domain in the `Origin` line, the `Response Header` will contain `Access-Control-Allow-Origin`
```bash
❯ ffuf -w '/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt' -u 'http://10.10.10.208' -H 'Origin: http://FUZZ.crossfit.htb' -mr 'Access-Control-Allow-Origin' -ignore-body

        /'___\  /'___\           /'___\
       /\ \__/ /\ \__/  __  __  /\ \__/
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
         \ \_\   \ \_\  \ \____/  \ \_\
          \/_/    \/_/   \/___/    \/_/

       v1.3.0 Kali Exclusive <3
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.10.208
 :: Wordlist         : FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt
 :: Header           : Origin: http://FUZZ.crossfit.htb
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Regexp: Access-Control-Allow-Origin
________________________________________________

ftp                     [Status: 200, Size: 10701, Words: 3427, Lines: 369]
www.ftp                 [Status: 200, Size: 10701, Words: 3427, Lines: 369]
:: Progress: [4989/4989] :: Job [1/1] :: 265 req/sec :: Duration: [0:00:17] :: Errors: 0 ::
```

## Subdomain found via FTP
[[25 - Gym-Club XSS]] Created an FTP user , upon logging in it looked like the users home was `/var/www`
```bash
lftp clarkey@10.10.10.208:~> dir
drwxrwxr-x    2 33       1002         4096 Sep 21  2020 development-test
drwxr-xr-x   13 0        0            4096 May 07  2020 ftp
drwxr-xr-x    9 0        0            4096 May 12  2020 gym-club
drwxr-xr-x    2 0        0            4096 May 01  2020 html
```
