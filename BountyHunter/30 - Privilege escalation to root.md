# Privilege escalation to root
## development user can run following command as sudo
```bash
development@bountyhunter:~$ sudo -l
Matching Defaults entries for development on bountyhunter:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User development may run the following commands on bountyhunter:
    (root) NOPASSWD: /usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py
```
Running command `sudo -l` tells tghat development user can run `/usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py` command as sudo.
![[Pasted image 20210916163151.png]]
## Analyzing contents o