# Privilege escalation to root
## Found `Krypter_Linux` file in development folder
```powershell
development@BREADCRUMBS C:\>cd Development

development@BREADCRUMBS C:\Development>dir
 Volume in drive C has no label.
 Volume Serial Number is 7C07-CD3A

 Directory of C:\Development

01/15/2021  05:03 PM    <DIR>          .
01/15/2021  05:03 PM    <DIR>          ..
11/29/2020  04:11 AM            18,312 Krypter_Linux
               1 File(s)         18,312 bytes
               2 Dir(s)   6,503,538,688 bytes free

development@BREADCRUMBS C:\Development>
```
## Transferring `Krypter_Linux` file to kali and analyzing in ghidra
Transferred `Krypter_Linux` file to kali via impacker smbserver
![[Pasted image 20210730205822.png]]


