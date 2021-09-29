# Initial shell as 'user' user
## Accessing admin.forge.htb gives `URL contains a blacklisted address!`
![[Pasted image 20210929184932.png]]
Accessing admin.forge.htb through `Upload from url` on http://forge.htb/upload gives `URL contains a blacklisted address!`, which needs to be bypassed
## Bypassing `URL contains a blacklisted address!` 
`URL contains a blacklisted address!` can be bypassed by using all capital letters of `admin.forge.htb` like `ADMIN.FORGE.HTB`.
![[Pasted image 20210929185612.png]]
## Seeing contents of link provided after accessing admin.forge.htb page
![[Pasted image 20210929190016.png]]
Seeing contents of link provided after accessing admin.forge.htb page reveals other links like `/announcements` and `/upload`.
## Seeing contents of admin.forge.htb/announcements
![[Pasted image 20210929192820.png]]
Seeing contents of admin.forge.htb/announcements gives internal ftp server credentials 
also reveals that the `/upload` endpoint now supports `ftp`, `ftps`, `http` and `https` protocols for uploading from url and also  `/upload` endpoint has been configured for easy scripting of uploads, and for uploading an image, one can simply pass a url with ?u=`<url>.
```html
<li>An internal ftp server has been setup with credentials as user:heightofsecurity123!</li>
        <li>The /upload endpoint now supports ftp, ftps, http and https protocols for uploading from url.</li>
        <li>The /upload endpoint has been configured for easy scripting of uploads, and for uploading an image, one can simply pass a url with ?u=&lt;url&gt;.</li>
```
## Using ftp protocol to get sensitive contents like id_rsa
![[Pasted image 20210929193800.png]]
```bash
~/Dropbox/Documents/htb/boxes/forge ❯ curl 'http://forge.htb/uploads/0bXhDe8mQpxZEzXKMJ5W' --output out
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   126  100   126    0     0    529      0 --:--:-- --:--:-- --:--:--   531
~/Dropbox/Documents/htb/boxes/forge ❯ cat out
drwxr-xr-x    3 1000     1000         4096 Aug 04 19:23 snap
-rw-r-----    1 0        1000           33 Sep 29 05:03 user.txt
```
