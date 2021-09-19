# Privilege Escalation to root
## Discovered local port 5555 on which adb in debug mode was running
Discovered local port 5555 on which adb in debug mode was running.
![[Pasted image 20210919190230.png]]
## Port forwarding port 5555 to kali VM via ssh tunnelling 
First of all SSH service was enabled on kali VM (explalined on [link](https://linuxize.com/post/how-to-setup-ssh-tunneling/). Then port 5555 on Explore machine was forwarded via ssh tunnelling to kali VM via command `ssh -R 5555:127.0.0.1:5555 -N -f kali@10.10.14.98 -p 2222`. 
![[Pasted image 20210919190913.png]]
## Now connec

