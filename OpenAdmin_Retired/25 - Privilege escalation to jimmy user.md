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