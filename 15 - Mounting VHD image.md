# Mounting VHD image
Mounting VHD image on local kali VM via guestmount tool using command `guestmount -a 9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd -m /dev/sda1 --ro ~/Dropbox/Documents/htb/boxes/Bastion/mount` [method](https://serverok.in/how-to-open-a-vhd-or-vhdx-file-in-linux)
![[Pasted image 20210714110734.png]]
## Copying SAM file to local kali VM
Copying SAM and SYSTEM file to local kali VM found in `/Windows/System32/config` .
![[Pasted image 20210714111426.png]]
## Using pwdump.py to dump hashes
