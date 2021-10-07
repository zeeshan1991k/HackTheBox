#  Initial shell as 'tony' user
## From [[Driver/10 - Common Enumeration]] it is discovered that SMB signing is disabled
From [[Driver/10 - Common Enumeration]] it is discovered that SMB signing is disabled.
```bash
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
Also from crackmapexec,
![[Pasted image 20211007183540.png]]
## If SMB signing is disabled we can do NTLM relay attack
If SMB signing is disabled we can do NTLM relay attack to get user hash.To perform this attack we 

