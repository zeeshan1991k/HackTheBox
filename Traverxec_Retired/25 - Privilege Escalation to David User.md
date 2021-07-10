# Privilege Escalation to David User
## Found hash in /var/nostromo/conf/.htpasswd
![[Pasted image 20210710130144.png]]
Which when cracked using google colab had the password `Nowonly4me` using the command `hashcat -m 500 hash.hash /content/drive/MyDrive/rockyou.txt` .
## Found nostromo aka nhttpd config file
Found nostromo aka nhttpd config file in `/var/nostromo/conf` , which indicates that there is some other directory named `public_www` which is accessbile by www-data 
in david's home directory . According to apache http server version 2.5 documentation [link](https://httpd.apache.org/docs/trunk/howto/public_html.html) , ` http://example.com/~rbowen/file.html` means `/home/rbowen/public_www/file.html`  meaning  `~david = /home/david/public_www` is accessible to www-data according to `/var/nostromo/conf`.
![[Pasted image 20210710132044.png]]
## Accessing /home/david/public_www directory
Accessing `/home/david/public_www directory` has the directory `protected-file-area`. 
![[Pasted image 20210710133354.png]]
Directory `protected-file-area` has TAR Archive file `backup-ssh-identity-files.tgz` .  
