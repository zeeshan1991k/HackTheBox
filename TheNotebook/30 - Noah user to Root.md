# Noah user to Root
## Sudo -l command 
`sudo -l` command showed that command `/usr/bin/docker exec -it webapp-dev01*`  can run as root
```bash
noah@thenotebook:/tmp$ sudo -l
Matching Defaults entries for noah on thenotebook:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User noah may run the following commands on thenotebook:
    (ALL) NOPASSWD: /usr/bin/docker exec -it webapp-dev01*
```
## Found installed docker version to find relevant exploit
Docker version was 18.06.0-ce.
```bash
noah@thenotebook:/tmp$ docker version                                                                                
Client:                                                                                                              
 Version:           18.06.0-ce                                                                                       
 API version:       1.38                                                                                             
 Go version:        go1.10.3                                                                                         
 Git commit:        0ffa825                                                                                          
 Built:             Wed Jul 18 19:09:54 2018                                                                         
 OS/Arch:           linux/amd64                                                                                      
 Experimental:      false                                             
 ```
 
## Exploit was found corresponding to installed docker version on victim machine
[Docker escape--runc container escape vulnerability (CVE-2019-5736)](https://programmersought.com/article/71085031667/)
[github page](https://github.com/Frichetten/CVE-2019-5736-PoC)
