# Found xlsx file in Edgar.Jacobs desktop folder in SMB share 
Found xlsx file in Edgar.Jacobs desktop folder in SMB share `RedirectedFolders$`.
```bash
~/Dropbox/Documents/htb/boxes/search ❯ smbclient -H \\\\10.10.11.129\\'RedirectedFolders$' -U 'Edgar.Jacobs'
Enter WORKGROUP\Edgar.Jacobs's password:
Try "help" to get a list of possible commands.
smb: \> cd Edgar.Jacobs
smb: \Edgar.Jacobs\> cd desktop
smb: \Edgar.Jacobs\desktop\> ls
  .                                 DRc        0  Mon Aug 10 15:02:16 2020
  ..                                DRc        0  Mon Aug 10 15:02:16 2020
  $RECYCLE.BIN                     DHSc        0  Fri Apr 10 01:05:29 2020
  desktop.ini                      AHSc      282  Mon Aug 10 15:02:16 2020
  Microsoft Edge.lnk                 Ac     1450  Fri Apr 10 01:05:03 2020
  Phishing_Attempt.xlsx              Ac    23130  Mon Aug 10 15:35:44 2020

                3246079 blocks of size 4096. 595033 blocks available
smb: \Edgar.Jacobs\desktop\>
```
![[Pasted image 20220225194809.png]]
## Opening this xlsx file revealed that one of the sheets is locked /protected
![[Pasted image 20220225195001.png]]
xlsx file revealed that one of the sheets `Passwords 01082020` is locked /protected
SO this protection has to be removed.
## Removing `Passwords 01082020` protection to reveal hidden columns
Removing `Passwords 01082020` protection to reveal hidden columns via using method on [link](https://andreafortuna.org/2018/02/14/how-to-unprotect-excel-worksheet-in-5-simple-steps/) 


