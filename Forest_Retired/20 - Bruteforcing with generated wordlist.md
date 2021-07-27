# Bruteforcing with generated wordlist
Before we do bruteforcing, we should make sure we dont lockout the target account.
So to know if there is lockout policy for target account , we perform `null authentication` attack via command `crackmapexec smb 10.10.10.161 --pass-pol -u '' -p ''`. 