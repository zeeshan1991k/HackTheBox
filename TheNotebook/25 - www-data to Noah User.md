# www-data to Noah User
## Found a note about potential backup directory
![[Pasted image 20210407060908.png]]
## Linpeas.sh script findings 
Linpeas.sh showed /var/backups folder had some archived home directory.

```bash
...[snip]...
[+] Backup folders
drwxr-xr-x 2 root root 4096 Apr  7 06:26 /var/backups
total 688
-rw-r--r-- 1 root root    51200 Apr  7 06:25 alternatives.tar.0
-rw-r--r-- 1 root root    33252 Feb 24 08:53 apt.extended_states.0
-rw-r--r-- 1 root root     3609 Feb 23 08:58 apt.extended_states.1.gz
-rw-r--r-- 1 root root     3621 Feb 12 06:52 apt.extended_states.2.gz
-rw-r--r-- 1 root root      437 Feb 12 06:17 dpkg.diversions.0
-rw-r--r-- 1 root root      172 Feb 12 06:52 dpkg.statoverride.0
-rw-r--r-- 1 root root   571460 Feb 24 08:53 dpkg.status.0
-rw------- 1 root root      693 Feb 17 13:18 group.bak
-rw------- 1 root shadow    575 Feb 17 13:18 gshadow.bak
-rw-r--r-- 1 root root     4373 Feb 17 09:02 home.tar.gz
-rw------- 1 root root     1555 Feb 12 06:24 passwd.bak
-rw------- 1 root shadow   1024 Feb 12 07:33 shadow.bak
...[snip]...
```
![[Pasted image 20210407061517.png]]
## Copied home.tar.gz to local kali vm
![[Pasted image 20210407062137.png]]
## ssh logged in as noah user via private key and got user flag
![[Pasted image 20210407062533.png]]