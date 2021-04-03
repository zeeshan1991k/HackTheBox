# Shell as WWW-Data
## LinPeas
### Crontab has issac running a script that has a vulnerablity[[45 - Issac]]
```bash
Crontab
*  *    * * *   isaac   /usr/bin/php /home/isaac/send_updates/send_updates.php
```

```bash
[+] Last time logon each user
Username         Port     From             Latest
root             tty1                      Mon Sep 21 04:46:55 -0400 2020
isaac            pts/0    10.10.10.2       Tue May 12 02:53:34 -0400 2020
ftpadm           pts/0    10.10.10.2       Tue May  5 12:37:36 -0400 2020
hank             pts/1    10.10.14.2       Mon Sep 21 05:46:24 -0400 2020
```

```bash
[+] Searching specific hashes inside files - less false positives (limit 70)
/etc/ansible/playbooks/adduser_hank.yml:$6$e20D6nUeTJOIyRio$A777Jj8tk5.sfACzLuIqqfZOCsKTVCfNEQIbH79nZf09mM.Iov/pzDCE8xNZZCM9MuHKMcjqNUd8QUEzC1CZG/
/var/www/ftp/database/factories/UserFactory.php:$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi
```

Hank's password is: powerpuffgirls (cracked)
