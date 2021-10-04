# Initial shell as www-data
## Discovered SQL Injection on http://10.10.11.101/administrative
Entering malicious entry in username field of http://10.10.11.101/administrative.
![[Pasted image 20211004103558.png]]
Logged in as admin user after entering malicious sql queiry in username field of http://10.10.11.101/administrative.
![[Pasted image 20211004103756.png]]
## Using [MySQL LOAD_FILE() function](https://www.w3resource.com/mysql/string-functions/mysql-load_file-function.php) to read files on system 
