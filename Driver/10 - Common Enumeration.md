# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
 ❯ sudo nmap -p- -sU -sS  --min-rate 10000 -oA nmap/driver 10.10.11.106
 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-04 15:38 PKT
Nmap scan report for 10.129.214.56
Host is up (0.23s latency).
Not shown: 65535 open|filtered ports, 65531 filtered ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
445/tcp  open  microsoft-ds
5985/tcp open  wsman

Nmap done: 1 IP address (1 host up) scanned in 27.97 seconds
```
All ports scan with service version and os detection.
```bash
❯ sudo nmap -p80,135,445,5985 -sC -sV -A -oA nmap/driver_all 10.10.11.106
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-04 15:41 PKT
Nmap scan report for 10.129.214.56
Host is up (0.21s latency).

PORT     STATE SERVICE      VERSION
80/tcp   open  http         Microsoft IIS httpd 10.0
| http-methods:
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp  open  msrpc        Microsoft Windows RPC
445/tcp  open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
5985/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Microsoft Windows Server 2008 R2 (91%), Microsoft Windows 10 1511 - 1607 (87%), Microsoft Windows 8.1 Update 1 (86%), Microsoft Windows Phone 7.5 or 8.0 (86%), FreeBSD 6.2-RELEASE (86%), Microsoft Windows 10 1607 (85%), Microsoft Windows 10 1511 (85%), Microsoft Windows 7 or Windows Server 2008 R2 (85%), Microsoft Windows Server 2008 R2 or Windows 8.1 (85%), Microsoft Windows Server 2008 R2 SP1 or Windows 8 (85%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: DRIVER; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 6h59m29s, deviation: 3s, median: 6h59m26s
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode:
|   2.10:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2021-10-04T17:41:25
|_  start_date: 2021-10-04T14:45:24

TRACEROUTE (using port 135/tcp)
HOP RTT       ADDRESS
1   213.88 ms 10.10.14.1
2   213.94 ms 10.10.11.106

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 57.56 seconds
```
## Added driver.htb to hosts file
![[Pasted image 20211004184017.png]]
