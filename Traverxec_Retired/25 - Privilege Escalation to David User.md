# Privilege Escalation to David User
## Found hash in /var/nostromo/conf/.htpasswd
![[Pasted image 20210710130144.png]]
Which when cracked using google colab had the password `Nowonly4me` using the command `hashcat -m 500 hash.hash /content/drive/MyDrive/rockyou.txt` .
# Found nostromo aka nhttpd config file
Found nostromo aka nhttpd config file in `/var/nostromo/conf` , which indicates that there is some other directory named `public_www` which is accessbile by www-data 
in david's home directory . 
![[Pasted image 20210710132044.png]]