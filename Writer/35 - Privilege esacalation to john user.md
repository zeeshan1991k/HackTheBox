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
## User kyle can execute `/var/www/writer2_project/manage.py` and write and execute `/etc/postfix/disclaimer`
User kyle can execute `/var/www/writer2_project/manage.py` and write and execute `/etc/postfix/disclaimer`
Running `/var/www/writer2_project/manage.py` gives following usage output stating we can email using ` sendtestemail` command.
```bash
kyle@writer:~$ python3 /var/www/writer2_project/manage.py

Type 'manage.py help <subcommand>' for help on a specific subcommand.

Available subcommands:

[auth]
    changepassword
    createsuperuser

[contenttypes]
    remove_stale_contenttypes

[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    sendtestemail
    shell
    showmigrations
    sqlflush
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    test
    testserver

[sessions]
    clearsessions

[staticfiles]
    collectstatic
    findstatic
    runserver
```
Contents of `/etc/postfix/disclaimer` are following.
```bash
kyle@writer:~$ cat /etc/postfix/disclaimer
#!/bin/sh
# Localize these.
INSPECT_DIR=/var/spool/filter
SENDMAIL=/usr/sbin/sendmail

# Get disclaimer addresses
DISCLAIMER_ADDRESSES=/etc/postfix/disclaimer_addresses

# Exit codes from <sysexits.h>
EX_TEMPFAIL=75
EX_UNAVAILABLE=69

# Clean up when done or when aborting.
trap "rm -f in.$$" 0 1 2 3 15

# Start processing.
cd $INSPECT_DIR || { echo $INSPECT_DIR does not exist; exit
$EX_TEMPFAIL; }

cat >in.$$ || { echo Cannot save mail to file; exit $EX_TEMPFAIL; }

# obtain From address
from_address=`grep -m 1 "From:" in.$$ | cut -d "<" -f 2 | cut -d ">" -f 1`

if [ `grep -wi ^${from_address}$ ${DISCLAIMER_ADDRESSES}` ]; then
  /usr/bin/altermime --input=in.$$ \
                   --disclaimer=/etc/postfix/disclaimer.txt \
                   --disclaimer-html=/etc/postfix/disclaimer.txt \
                   --xheader="X-Copyrighted-Material: Please visit http://www.company.com/privacy.htm" || \
                    { echo Message content rejected; exit $EX_UNAVAILABLE; }
fi

$SENDMAIL "$@" <in.$$

exit $?
```
Sending email via `sendtestemail` command in `/var/www/writer2_project/manage.py ` to `kyle@writer.htb` and `root@writer.htb` triggers `/etc/postfix/disclaimer` script which is running as john as explained in [link](https://www.howtoforge.com/add-disclaimers-to-outgoing-emails-with-altermime-postfix-debian-etch) So we can copy public key of kali vm in john's `.ssh`'s authorized_keys file so that when we send email via `sendtestemail` to trigger `/etc/postfix/disclaimer` script so that kali vm public is copied to `/home/john/.ssh/authorized_keys` and we can ssh log in as john user.
## Editing `/etc/postfix/disclaimer` and sending email via `sendtestemail` to trigger  `/etc/postfix/disclaimer` and copy kali vm public key and ssh log in as john user
![[Pasted image 20211004132956.png]]






