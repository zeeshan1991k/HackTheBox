# Bruteforcing with generated wordlist
## Knowing if Bruteforcing does not lockout accounts
Before we do bruteforcing, we should make sure we dont lockout the target account.
So to know if there is lockout policy for target account , we perform `null authentication` attack via command `crackmapexec smb 10.10.10.161 --pass-pol -u '' -p ''` which gives the following output.
![[Pasted image 20210727100616.png]]
Which tells that `null authentication` is enabled and allows for domain enumeration and we can get bunch of information. 
## Bruteforcing using crac




