# Privilege escalation to jimmy user
Used mysqli database password  got in [[OpenAdmin_Retired/5 - Loot]] to ssg log in as `jimmy` user.
![[Pasted image 20210706025847.png]]
## Found another subdomain in www directory via jimmy user
Found another subdomain in www directory named internal which contained three
php files
![[Pasted image 20210707024051.png]]
## Found another service running on 127.0.0.1:52846
Found another service(possibly another website) running on 127.0.0.1:52846
![[Pasted image 20210707031911.png]]
## Port forwarding service/website on port 52846 to kali via chisel
 Port forwarding service/website on port 52846 to kali via chisel.
 ![[Pasted image 20210707032551.png]]
 ## Found hash for service running on 127.0.0.1:52846
 Found sha512 hash for service/webpage running on 127.0.0.1:52846.
```bash
$ cat index.php | grep password
         .form-signin input[type="password"] {
            if (isset($_POST['login']) && !empty($_POST['username']) && !empty($_POST['password'])) {
              if ($_POST['username'] == 'jimmy' && hash('sha512',$_POST['password']) == '00e302ccdcf1c60b8ad50ea50cf72b939705f49f40f0dc658801b4680b7d758eebdc2e9f9ba8ba3ef8a8bb9a796d34ba2e856838ee9bdde852b8ec3b3a0523b1') {
                  $msg = 'Wrong username or password.';
            <input type = "password" class = "form-control"
               name = "password" required>
```
Cracked this hash and found password as Revealed.
![[Pasted image 20210707033318.png]]
## Found ssh private key for joanna user
 Found ssh private key for joanna user after entering credentials jimmy:Revealed on login page.
![[Pasted image 20210707033759.png]]