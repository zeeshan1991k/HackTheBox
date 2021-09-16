# Initial shell as development user
## Intercepting POST request of url http://10.10.11.100/log_submit.php 
Upon clicking submit button , it makes POST request to  `/tracker_diRbPr00f314.php`.
![[Pasted image 20210916155240.png]]
## Form contents in base64 encoded XML
POST request to  `/tracker_diRbPr00f314.php` contains `data` as POST parameter which contains contents of form in base64 encoded XML.
![[Pasted image 20210916155801.png]]
## Discovered XXE vulnerability
Discovered XXE vulnerability and read /etc/passwd contents which contained development as user
![[Pasted image 20210916160444.png]]
## Used PHP wrapper to read db.php file contents
Used XXE payload to read contents of db.php
![[Pasted image 20210916161018.png]]
![[Pasted image 20210916161151.png]]
## Used above credentials to get shell as development user

