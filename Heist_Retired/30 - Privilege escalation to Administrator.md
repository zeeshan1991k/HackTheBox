# Privilege escalation to Administrator
## Firefox Credential files found 
Firefox Credential file found in  `C:\Users\Chase\AppData\Roaming\Mozilla\Firefox\Profiles\77nc64t5.default\key4.db`, which indicates that firefox process is owned by user `chase` and some  credentials in memory can be dumped via from firefox process.
![[Pasted image 20210712100645.png]]
## Firefox process found in processes list
Issuing command `Get-Process | where {$_.ProcessName -notlike "svchost*"} | ft ProcessName, Id` gave list of processes which included `firefox`.
![[Pasted image 20210712101521.png]]
## Getting memory dump of firefox processes via procdump utility
Transferred `procdump.exe` file to `Heist` machine .
![[Pasted image 20210712104544.png]]
Dumping contents of all firefox processes.
![[Pasted image 20210712104744.png]]
