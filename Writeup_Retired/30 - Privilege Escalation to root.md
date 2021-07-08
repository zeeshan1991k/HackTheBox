# Privilege Escalation to root
Running pspy64 python script on writeup machine(victim machine) revealed that some command is being run as root when someone ssh logs screenshot of which is shown below.
![[Pasted image 20210708154435.png]]
In this command `sh -c /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin run-parts --lsbsysinit /etc/update-motd.d > /run/motd.dynamic.new`, `run-parts`  command will run according to precedence rule which means that `run-parts` command will run from directory which comes first in PATH . 
It was found that /usr/local/sbin directory has write permissions for users other than root.
![[Pasted image 20210708155449.png]]
So we can write reverse-shell script with the same name as `run-parts` command so that when some one ssh logs in , command `run-parts` will be executed from `/usr/local/sbin` instead of its original directory to get reverse shell as root.
![[Pasted image 20210708155917.png]]
Got reverse shell as root by ssh logging in and triggering the `run-parts` from `/usr/local/sbin` directory.
![[Pasted image 20210708160610.png]]
