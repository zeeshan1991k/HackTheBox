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
## Private key encrypted with passphrase
Private key encrypted with passphrase so it first its hash was needed to be extracted by JohnTheRipper python script ssh2john which is done as follows.
```bash
 python /home/kali/JohnTheRipper/run/ssh2john.py joanna_user_private_key.pem > id_rsa.hash                                         ~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/openadmin_retired ❯ cat id_rsa.hash                                                                                                                                         
joanna_user_private_key.pem:$sshng$1$16$2AF25344B8391A25A9B318F3FD767D6D$1200$906d14608706c9ac6ea6342a692d9ed47a9b87044b94d72d5b61df25e68a5235991f8bac883f40b539c829550ea5937c69dfd2b4c589f8c910e4c9c030982541e51b4717013fafbe1e1db9d6331c83cca061cc7550c0f4dd98da46ec1c7f460e4a135b6f1f04bafaf66a08db17ecad8a60f25a1a095d4f94a530f9f0bf9222c6736a5f54f1ff93c6182af4ad8a407044eb16ae6cd2a10c92acffa6095441ed63215b6126ed62de25b2803233cc3ea533d56b72d15a71b291547983bf5bee5b0966710f2b4edf264f0909d6f4c0f9cb372f4bb323715d17d5ded5f83117233976199c6d86bfc28421e217ccd883e7f0eecbc6f227fdc8dff12ca87a61207803dd47ef1f2f6769773f9cb52ea7bb34f96019e00531fcc267255da737ca3af49c88f73ed5f44e2afda28287fc6926660b8fb0267557780e53b407255dcb44899115c568089254d40963c8511f3492efe938a620bde879c953e67cfb55dbbf347ddd677792544c3bb11eb0843928a34d53c3e94fed25bff744544a69bc80c4ffc87ffd4d5c3ef5fd01c8b4114cacde7681ea9556f22fc863d07a0f1e96e099e749416cca147add636eb24f5082f9224e2907e3464d71ae711cf8a3f21bd4476bf98c633ff1bbebffb42d24544298c918a7b14c501d2c43534b8428d34d500537f0197e75a4279bbe4e8d2acee3c1586a59b28671e406c0e178b4d29aaa7a478b0258bde6628a3de723520a66fb0b31f1ea5bf45b693f868d47c2d89692920e2898ccd89710c42227d31293d9dad740791453ec8ebfb26047ccca53e0a200e9112f345f5559f8ded2f193feedd8c1db6bd0fbfa5441aa773dd5c4a60defe92e1b7d79182af16472872ab3c222bdd2b5f941604b7de582b08ce3f6635d83f66e9b84e6fe9d3eafa166f9e62a4cdc993d42ed8c0ad5713205a9fc7e5bc87b2feeaffe05167a27b04975e9366fa254adf511ffd7d07bc1f5075d70b2a7db06f2224692566fb5e8890c6e39038787873f21c52ce14e1e70e60b8fca716feb5d0727ac1c355cf633226c993ca2f16b95c59b3cc31ac7f641335d80ff1ad3e672f88609ec5a4532986e0567e169094189dcc82d11d46bf73bc6c48a05f84982aa222b4c0e78b18cceb15345116e74f5fbc55d407ed9ba12559f57f37512998565a54fe77ea2a2224abbddea75a1b6da09ae3ac043b6161809b630174603f33195827d14d0ebd64c6e48e0d0346b469d664f89e2ef0e4c28b6a64acdd3a0edf8a61915a246feb25e8e69b3710916e494d5f482bf6ab65c675f73c39b2c2eecdca6709188c6f36b6331953e3f93e27c987a3743eaa71502c43a807d8f91cdc4dc33f48b852efdc8fcc2647f2e588ae368d69998348f0bfcfe6d65892aebb86351825c2aa45afc2e6869987849d70cec46ba951c864accfb8476d5643e7926942ddd8f0f32c296662ba659e999b0fb0bbfde7ba2834e5ec931d576e4333d6b5e8960e9de46d32daa5360ce3d0d6b864d3324401c4975485f1aef6ba618edb12d679b0e861fe5549249962d08d25dc2dde517b23cf9a76dcf482530c9a34762f97361dd95352de4c82263cfaa90796c2fa33dd5ce1d889a045d587ef18a5b940a2880e1c706541e2b523572a8836d513f6e688444af86e2ba9ad2ded540deadd9559eb56ac66fe021c3f88c2a1a484d62d602903793d10d
```
## Extracted hash cracked using JohnTheRipper
Extracted hash cracked using JohnTheRipper.
```bash
❯ john --wordlist=/home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt id_rsa.hash                                                                01:02:09 PM

Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
bloodninjas      (joanna_user_private_key.pem)
1g 0:00:00:06 DONE (2021-07-07 13:02) 0.1592g/s 2283Kp/s 2283Kc/s 2283KC/sa6_123..*7¡Vamos!
Session completed
```


