# Initial shell as www-data
## Discovered SQL Injection on http://10.10.11.101/administrative
Entering malicious entry in username field of http://10.10.11.101/administrative.
![[Pasted image 20211004103558.png]]
Logged in as admin user after entering malicious sql queiry in username field of http://10.10.11.101/administrative.
![[Pasted image 20211004103756.png]]
## Using [MySQL LOAD_FILE() function](https://www.w3resource.com/mysql/string-functions/mysql-load_file-function.php) to read files on system 
We can run combine commands via `UNION SELECT` keywords but `UNION SELECT` QUERY must have same number of columns . As table storing users (have only one user as admin) have 6 columns so we will have the `UNION SELECT` selecting 6 columns with command `admin' UNION SELECT 1,LOAD_FILE("/etc/passwd"),3,4,5,6#` . Output of this query in burpsuite is shown below.
![[Pasted image 20211004105357.png]]
Reading 