# Finding Shortest Paths to Domain Admins from Owned Principals(svc-alfreso user)
First of all we will mark svc-alfresco as owned.
![[Pasted image 20210727192051.png]]
Then we will find Shortest Paths to Domain Admins from Owned Principals(svc-alfreso user)
![[Pasted image 20210727192533.png]]
As svc-alfresco user is member of Account Operators group, so The Account Operators group grants limited account creation privileges to a user. Members of this group can create and modify most types of accounts, including those of users, local groups, and global groups, and members can log in locally to domain controllers.

Members of the Account Operators group cannot manage the Administrator user account, the user accounts of administrators, or the [Administrators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-admins), [Server Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-serveroperators), [Account Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-accountoperators), [Backup Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-backupoperators), or [Print Operators](https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups#bkmk-printoperators) groups. Members of this group cannot modify user rights.
