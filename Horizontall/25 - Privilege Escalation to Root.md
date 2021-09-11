# Privilege Escalation to Root
## Found a port  running locally on port 8000
Found a port running locally on port 8000 via `linpeas.sh`. 
![[Pasted image 20210911154613.png]]
## Forwarded this port to kali VM via chisel 
 Forwarded this port to kali VM via chisel to see what service was running on this port. 
 ## Found that PHP Laravel version 8 was running on this port
 It turned out that PHP laravel version 8 was running on this port.
![[Pasted image 20210911155004.png]]
![[Pasted image 20210911155346.png]]
## Found that PHP Laravel is running in DEBUG mode
Found that PHP Laravel is running in DEBUG mode.
![[Pasted image 20210911160123.png]]




