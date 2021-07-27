# Uploading files to forest machine via smbserver
First made a directory named `smb` , cd into that directory and run the command `mpacket-smbserver clarkey $(pwd) -smb2support -user clark -password clark1234` to start smbserver in `smb` directory.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/forest_retired/smb ❯ impacket-smbserver clarkey(smbshare name) $(pwd){make present directory as share folder} -smb2support -user clark(username for smbshare) -password clark1234(password for smbshare) 
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```
