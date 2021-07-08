# Privilege Escalation to root
## Using pspy64 to monitor processes
Running pspy64 python script on writeup machine(victim machine) revealed that some command is being run as root when someone ssh logs screenshot of which is shown below.
![[Pasted image 20210708154435.png]]
In this command `sh -c /usr/bin/env -i PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin run-parts --lsbsysinit /etc/update-motd.d > /run/motd.dynamic.new`, `run-parts`  command will run according to precedence rule which means that `run-parts` command will run from directory which comes first in PATH . 
It was found that /usr/local/sbin directory has write permissions for users other than root.

