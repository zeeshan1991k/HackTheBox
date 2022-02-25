# Using hope sharp credentials to access smb share
```bash
~/Dropbox/Documents/htb/boxes/search ❯ crackmapexec smb 10.10.11.129 -u 'hope.sharp' -p 'IsolationIsKey?' --shares
SMB         10.10.11.129    445    RESEARCH         [*] Windows 10.0 Build 17763 x64 (name:RESEARCH) (domain:search.htb) (signing:True) (SMBv1:False)
SMB         10.10.11.129    445    RESEARCH         [+] search.htb\hope.sharp:IsolationIsKey?
SMB         10.10.11.129    445    RESEARCH         [+] Enumerated shares
SMB         10.10.11.129    445    RESEARCH         Share           Permissions     Remark
SMB         10.10.11.129    445    RESEARCH         -----           -----------     ------
SMB         10.10.11.129    445    RESEARCH         ADMIN$                          Remote Admin
SMB         10.10.11.129    445    RESEARCH         C$                              Default share
SMB         10.10.11.129    445    RESEARCH         CertEnroll      READ            Active Directory Certificate Services share
SMB         10.10.11.129    445    RESEARCH         helpdesk
SMB         10.10.11.129    445    RESEARCH         IPC$            READ            Remote IPC
SMB         10.10.11.129    445    RESEARCH         NETLOGON        READ            Logon server share
SMB         10.10.11.129    445    RESEARCH         RedirectedFolders$ READ,WRITE
SMB         10.10.11.129    445    RESEARCH         SYSVOL          READ            Logon server share
```
![[Pasted image 20220225185050.png]]
## Accessing shares
```bash
~/Dropbox/Documents/htb/boxes/search ❯ smbclient -H \\\\10.10.11.129\\'RedirectedFolders$' -U 'hope.sharp'
Enter WORKGROUP\hope.sharp's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  Dc        0  Fri Feb 25 18:48:17 2022
  ..                                 Dc        0  Fri Feb 25 18:48:17 2022
  abril.suarez                       Dc        0  Tue Apr  7 23:12:58 2020
  Angie.Duffy                        Dc        0  Fri Jul 31 18:11:32 2020
  Antony.Russo                       Dc        0  Fri Jul 31 17:35:32 2020
  belen.compton                      Dc        0  Tue Apr  7 23:32:31 2020
  Cameron.Melendez                   Dc        0  Fri Jul 31 17:37:36 2020
  chanel.bell                        Dc        0  Tue Apr  7 23:15:09 2020
  Claudia.Pugh                       Dc        0  Fri Jul 31 18:09:08 2020
  Cortez.Hickman                     Dc        0  Fri Jul 31 17:02:04 2020
  dax.santiago                       Dc        0  Tue Apr  7 23:20:08 2020
  Eddie.Stevens                      Dc        0  Fri Jul 31 16:55:34 2020
  edgar.jacobs                       Dc        0  Fri Apr 10 01:04:11 2020
  Edith.Walls                        Dc        0  Fri Jul 31 17:39:50 2020
  eve.galvan                         Dc        0  Tue Apr  7 23:23:13 2020
  frederick.cuevas                   Dc        0  Tue Apr  7 23:29:22 2020
  hope.sharp                         Dc        0  Thu Apr  9 19:34:41 2020
  jayla.roberts                      Dc        0  Tue Apr  7 23:07:00 2020
  Jordan.Gregory                     Dc        0  Fri Jul 31 18:01:06 2020
  payton.harmon                      Dc        0  Fri Apr 10 01:11:39 2020
  Reginald.Morton                    Dc        0  Fri Jul 31 16:44:32 2020
  santino.benjamin                   Dc        0  Tue Apr  7 23:10:25 2020
  Savanah.Velazquez                  Dc        0  Fri Jul 31 17:21:42 2020
  sierra.frye                        Dc        0  Thu Nov 18 06:01:46 2021
  trace.ryan                         Dc        0  Fri Apr 10 01:14:26 2020

                3246079 blocks of size 4096. 595585 blocks available
smb: \>
```
![[Pasted image 20220225185321.png]]

