# Privilege Escalation to root
User joanna can run following commands as sudo.
```bash
$ sudo -l
Matching Defaults entries for joanna on openadmin:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User joanna may run the following commands on openadmin:
    (ALL) NOPASSWD: /bin/nano /opt/priv
```
When executed `sudo /bin/nano /opt/priv` , nano editor is opened which can read and execute commands as root. So running nano as sudo kali(my machine) public key can be copied to /root/.ssh/authorized keys and then can be ssh logged in as root.Procedure is shown below.
Nano 
