# Privilege Escalation to root
## Found server-stats.sh in user david bin directory
Found `server-stats.sh` in user `david` `bin` directory which had following contents
```bash
$ cat server-stats.sh
#!/bin/bash

cat /home/david/bin/server-stats.head
echo "Load: `/usr/bin/uptime`"
echo " "
echo "Open nhttpd sockets: `/usr/bin/ss -H sport = 80 | /usr/bin/wc -l`"
echo "Files in the docroot: `/usr/bin/find /var/nostromo/htdocs/ | /usr/bin/wc -l`"
echo " "
echo "Last 5 journal log lines:"
/usr/bin/sudo /usr/bin/journalctl -n5 -unostromo.service | /usr/bin/cat
```
This script is running `journalctl` as sudo and printing first five lines of `unostromo.service` log . 

