# Getting shell as jkr user
## Found exploit related to CMS Made simple [[Writeup_Retired/15 - Webpages]]
Used this exploit in python to gain username and password [CMS Made Simple < 2.2.10 - SQL Injection](https://www.exploit-db.com/exploits/46635) 
Command Used `python2 exploit.py -u http://writeup.htb/writeup --crack -w /home/kali/Dropbox/Documents/htb/boxes/compromised/rockyou.txt` to crack username and password
![[Pasted image 20210707110832.png]]
## ssh logged in as jkr user using above credentials
![[Pasted image 20210707122003.png]]

