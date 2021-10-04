#  Privilege esacalation to john user
## Running id command reveals that kyle user is part of filter and smbgroup 
![[Pasted image 20211004130048.png]]
## Running find command to know files owned/writeable/accessible by smbgroup and filter group
```bash
kyle@writer:~$ find /var/www/writer2_project/*.py -group smbgroup 2>/dev/null
/var/www/writer2_project/manage.py
kyle@writer:~$ find / -group filter 2>/dev/null
/etc/postfix/disclaimer
/var/spool/filter
kyle@writer:~$ ls -la /var/www/writer2_project/manage.py
-r-xr-sr-x 1 www-data smbgroup 806 Oct  4 08:02 /var/www/writer2_project/manage.py
kyle@writer:~$ ls -la /etc/postfix/disclaimer
-rwxrwxr-x 1 root filter 1021 Oct  4 08:02 /etc/postfix/disclaimer
kyle@writer:~$ ls -la /var/spool/filter
total 8
drwxr-x--- 2 filter filter 4096 May 13 22:31 .
drwxr-xr-x 7 root   root   4096 May 18 16:54 ..
kyle@writer:~$
````
![[Pasted image 20211004130545.png]]
