# Shell as Hank
## Credentials to databases
No real juicy info in either FTPhosting or Crossfit databases.

## PHP Cron Script
Isaac runs send_updates.php every minute, which is vulnerable to a command injection vulnerability due to outdated library [mikeheartl-shellcommand](https://github.com/mikehaertl/php-shellcommand/issues/44). Data in the email column can be used to execute code, this is triggered only if file exists in /var/ftp/messages/ ($msg_dir).

Below is  /home/isaac/send_updates/send_updates.php
```php
<?php
/***************************************************
 * Send email updates to users in the mailing list *
 ***************************************************/
 // Dont have permission
require("vendor/autoload.php");
require("includes/functions.php");
require("includes/db.php");
require("includes/config.php");
use mikehaertl\shellcommand\Command;

if($conn)
{
    // Unknown Directory
	$fs_iterator = new FilesystemIterator($msg_dir);

    foreach ($fs_iterator as $file_info)
    {
        if($file_info->isFile())
        {
            $full_path = $file_info->getPathname();
            $res = $conn->query('SELECT email FROM users');
            while($row = $res->fetch_array(MYSQLI_ASSOC))
            {
                // Probably Vulnerable
				$command = new Command('/usr/bin/mail');
                $command->addArg('-s', 'CrossFit Club Newsletter', $escape=true);
                $command->addArg($row['email'], $escape=true);

                $msg = file_get_contents($full_path);
                $command->setStdIn('test');
                $command->execute();
            }
        }
        unlink($full_path);
    }
}

cleanup();
?>
```
php-shellcommand has a [bug]

## Command Injection to Issac
Creating a file with ftpadm account via ftp + Putting shell in crossfit database users table email column
