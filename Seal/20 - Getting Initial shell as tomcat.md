#  Getting Initial shell as tomcat
## Made account on gitbucket on http://10.10.10.250:8080/
![[Pasted image 20210928100501.png]]
## Logging in gitbucket as newly created user 'clarkey'
![[Pasted image 20210928100649.png]]
## http://10.10.10.250/manager/html giving 403 forbidden
http://10.10.10.250/manager/html giving 403 forbidden. We need to bypass 403 forbidden to access http://10.10.10.250/manager/html.
![[Pasted image 20210928100947.png]]
## http://10.10.10.250/manager/status is asking for password 
http://10.10.10.250/manager/status is asking for password which is found in tomcat-users.xml
![[Pasted image 20210928101336.png]]
## FInding  password in commit history of tomcat-users.xml
Found password in commit history of tomcat-users.xml.
![[Pasted image 20210928101714.png]]
## Using above password to access http://10.10.10.250/manager/status
