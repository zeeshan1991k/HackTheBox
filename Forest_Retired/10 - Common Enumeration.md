# Common Enumeration
## Note -> Windows/Linux TTL
*  Windows default TTL is 128. Everytime packet traverses a router this decrements by one because TTL stands for Time To LIve.
*  Linux Default TTL is 64.
*  If TTL is greater is than 128, chances are it is some type of network infrastructure.


## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/forest 10.10.10.161
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-24 13:17 PKT
Nmap scan report for 10.10.10.161
Host is up (0.12s latency).
Not shown: 65511 closed ports
PORT      STATE SERVICE
53/tcp    open  domain
88/tcp    open  kerberos-sec
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
389/tcp   open  ldap
445/tcp   open  microsoft-ds
464/tcp   open  kpasswd5
593/tcp   open  http-rpc-epmap
636/tcp   open  ldapssl
3268/tcp  open  globalcatLDAP
3269/tcp  open  globalcatLDAPssl
5985/tcp  open  wsman
9389/tcp  open  adws
47001/tcp open  winrm
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49671/tcp open  unknown
49676/tcp open  unknown
49677/tcp open  unknown
49684/tcp open  unknown
49706/tcp open  unknown
49928/tcp open  unknown
```
Detected ports scan with service version and os detection.
```bash
❯ sudo nmap -p53,88,135,139,389,445,464,593,636,3268,3269,5985,9389,47001,49664,49665,49666,49667,49671,49676,49677,49684,49706,49928 -A  -oA nmap/forest_version 10.10.10.161
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-24 13:19 PKT
Nmap scan report for 10.10.10.161
Host is up (0.11s latency).

PORT      STATE SERVICE      VERSION
53/tcp    open  domain       Simple DNS Plus
88/tcp    open  kerberos-sec Microsoft Windows Kerberos (server time: 2021-07-24 08:26:02Z)
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
389/tcp   open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds (workgroup: HTB)
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap         Microsoft Windows Active Directory LDAP (Domain: htb.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf       .NET Message Framing
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49671/tcp open  msrpc        Microsoft Windows RPC
49676/tcp open  ncacn_http   Microsoft Windows RPC over HTTP 1.0
49677/tcp open  msrpc        Microsoft Windows RPC
49684/tcp open  msrpc        Microsoft Windows RPC
49706/tcp open  msrpc        Microsoft Windows RPC
49928/tcp open  msrpc        Microsoft Windows RPC
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Microsoft Windows Server 2016 build 10586 - 14393 (96%), Microsoft Windows Server 2016 (95%), Microsoft Windows 10 1507 (93%), Microsoft Windows 10 1507 - 1607 (93%), Microsoft Windows Server 2012 (93%), Microsoft Windows Server 2012 R2 (93%), Microsoft Windows Server 2012 R2 Update 1 (93%), Microsoft Windows 7, Windows Server 2012, or Windows 8.1 Update 1 (93%), Microsoft Windows Vista SP1 - SP2, Windows Server 2008 SP2, or Windows 7 (93%), Microsoft Windows 10 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: FOREST; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 2h26m51s, deviation: 4h02m30s, median: 6m50s
| smb-os-discovery:
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: FOREST
|   NetBIOS computer name: FOREST\x00
|   Domain name: htb.local
|   Forest name: htb.local
|   FQDN: FOREST.htb.local
|_  System time: 2021-07-24T01:26:58-07:00
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: required
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2021-07-24T08:26:59
|_  start_date: 2021-07-23T05:27:23

TRACEROUTE (using port 443/tcp)
HOP RTT       ADDRESS
1   111.15 ms 10.10.14.1
2   110.89 ms 10.10.10.161

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 75.28 seconds
```
## SMB
Anonymous success but no shares
## DNS
Could not leak host name.
```bash
❯ nslookup
> server 10.10.10.161
Default server: 10.10.10.161
Address: 10.10.10.161#53
> 127.0.0.1
1.0.0.127.in-addr.arpa  name = localhost.
> 127.0.0.2
** server can't find 2.0.0.127.in-addr.arpa: NXDOMAIN
> 10.10.10.161

;; connection timed out; no servers could be reached

>
```
## LDAP search
This would give us the DN(Domain Name).
![[Pasted image 20210726124952.png]]
```bash
 ❯ ldapsearch -h 10.10.10.161 -x -s base namingcontexts
# extended LDIF
#
# LDAPv3
# base <> (default) with scope baseObject
# filter: (objectclass=*)
# requesting: namingcontexts
#

#
dn:
namingContexts: DC=htb,DC=local
namingContexts: CN=Configuration,DC=htb,DC=local
namingContexts: CN=Schema,CN=Configuration,DC=htb,DC=local
namingContexts: DC=DomainDnsZones,DC=htb,DC=local
namingContexts: DC=ForestDnsZones,DC=htb,DC=local

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```
This command `ldapsearch -h 10.10.10.161 -x -b "DC=htb,DC=local"` will give all the LDAP information of what we can query. Its output has been saved to `ldap-anonymous.out`.
This  command `ldapsearch -h 10.10.10.161 -x -b "DC=htb,DC=local" '(objectClass=Person)'` will only dump things that have `object class` of `People`.
```bash
# Santi Rodriguez, Developers, Information Technology, Employees, htb.local
dn: CN=Santi Rodriguez,OU=Developers,OU=Information Technology,OU=Employees,DC
 =htb,DC=local
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: user
cn: Santi Rodriguez
sn: Rodriguez
givenName: Santi
distinguishedName: CN=Santi Rodriguez,OU=Developers,OU=Information Technology,
 OU=Employees,DC=htb,DC=local
instanceType: 4
whenCreated: 20190920230255.0Z
whenChanged: 20210725070807.0Z
displayName: Santi Rodriguez
uSNCreated: 28837
uSNChanged: 10837300
name: Santi Rodriguez
objectGUID:: VSlmUT29FkGHUAJ12EnggA==
userAccountControl: 66048
badPwdCount: 3089
codePage: 0
countryCode: 0
badPasswordTime: 132715236307421854
lastLogoff: 0
lastLogon: 0
pwdLastSet: 132134941751348277
primaryGroupID: 513
objectSid:: AQUAAAAAAAUVAAAALB4ltxV1shXFsPNPgAQAAA==
accountExpires: 9223372036854775807
logonCount: 0
sAMAccountName: santi
sAMAccountType: 805306368
userPrincipalName: santi@htb.local
objectCategory: CN=Person,CN=Schema,CN=Configuration,DC=htb,DC=local
dSCorePropagationData: 20210726052755.0Z
dSCorePropagationData: 20210726052755.0Z
dSCorePropagationData: 20210726052755.0Z
dSCorePropagationData: 20210726052755.0Z
dSCorePropagationData: 16010101000000.0Z
```
From above info of `Santi Rodriguez`, `whenCreated: 20190920230255.0Z` is important which can be interpreted as `2019-09-20-23:02:55` , we can also see `logonCount`, `userPrincipalName` (potential email address), `badPwdCount`, `pwdLastSet: 132134941751348277`. We can convert LDAP timestamp `132134941751348277` to human readable time/date via this [website](https://www.epochconverter.com/ldap) which gave the following output.
![[Pasted image 20210726132516.png]]
All this information is useful when doing password sprays.
`sAMAccountName` is the username of windows login prompt when we enter username and password to login to windows.
`ldapsearch -h 10.10.10.161 -x -b "DC=htb,DC=local" '(objectClass=Person)' sAMAccountName | grep sAMAccountName`
Above command will give `sAMAccountName`(All users on AD) only. This users' list is for Password spraying.
```bash
❯ ldapsearch -h 10.10.10.161 -x -b "DC=htb,DC=local" '(objectClass=Person)' sAMAccountName | grep sAMAccountName
# requesting: sAMAccountName
sAMAccountName: Guest
sAMAccountName: DefaultAccount
sAMAccountName: FOREST$
sAMAccountName: EXCH01$
sAMAccountName: $331000-VK4ADACQNUCA
sAMAccountName: SM_2c8eef0a09b545acb
sAMAccountName: SM_ca8c2ed5bdab4dc9b
sAMAccountName: SM_75a538d3025e4db9a
sAMAccountName: SM_681f53d4942840e18
sAMAccountName: SM_1b41c9286325456bb
sAMAccountName: SM_9b69f1b9d2cc45549
sAMAccountName: SM_7c96b981967141ebb
sAMAccountName: SM_c75ee099d0a64c91b
sAMAccountName: SM_1ffab36a2f5f479cb
sAMAccountName: HealthMailboxc3d7722
sAMAccountName: HealthMailboxfc9daad
sAMAccountName: HealthMailboxc0a90c9
sAMAccountName: HealthMailbox670628e
sAMAccountName: HealthMailbox968e74d
sAMAccountName: HealthMailbox6ded678
sAMAccountName: HealthMailbox83d6781
sAMAccountName: HealthMailboxfd87238
sAMAccountName: HealthMailboxb01ac64
sAMAccountName: HealthMailbox7108a4e
sAMAccountName: HealthMailbox0659cc1
sAMAccountName: sebastien
sAMAccountName: lucinda
sAMAccountName: andy
sAMAccountName: mark
sAMAccountName: santi
```
Now Extracting account names only from above list by command `ldapsearch -h 10.10.10.161 -x -b "DC=htb,DC=local" '(objectClass=Person)' sAMAccountName | grep sAMAccountName | awk '{print $2}'`
```bash
requesting
Guest (Excluded for password spraying)
DefaultAccount (Excluded for password spraying)
FOREST$ (Accounts ending with $ are generated by Active Directory themselves, so they are called machine accounts,so will not be password spraying against them)
EXCH01$ (=)
$331000-VK4ADACQNUCA (Excluded for password spraying because these usernames are generated by exchange)
SM_2c8eef0a09b545acb (Excluded for password spraying =)
SM_ca8c2ed5bdab4dc9b (Excluded for password spraying =)
SM_75a538d3025e4db9a (Excluded for password spraying =)
SM_681f53d4942840e18 (Excluded for password spraying =)
SM_1b41c9286325456bb (Excluded for password spraying =)
SM_9b69f1b9d2cc45549 (Excluded for password spraying =)
SM_7c96b981967141ebb (Excluded for password spraying =)
SM_c75ee099d0a64c91b (Excluded for password spraying =)
SM_1ffab36a2f5f479cb (Excluded for password spraying =)
HealthMailboxc3d7722 (Excluded for password spraying =)
HealthMailboxfc9daad (Excluded for password spraying =)
HealthMailboxc0a90c9 (Excluded for password spraying =)
HealthMailbox670628e (Excluded for password spraying =)
HealthMailbox968e74d (Excluded for password spraying =)
HealthMailbox6ded678 (Excluded for password spraying =)
HealthMailbox83d6781 (Excluded for password spraying =)
HealthMailboxfd87238 (Excluded for password spraying =)
HealthMailboxb01ac64 (Excluded for password spraying =)
HealthMailbox7108a4e (Excluded for password spraying =)
HealthMailbox0659cc1 (Excluded for password spraying =)
sebastien
lucinda
andy
mark
santi
```
Saving above list to file named `userlist.ldap` via command `ldapsearch -h 10.10.10.161 -x -b "DC=htb,DC=local" '(objectClass=Person)' sAMAccountName | grep sAMAccountName | awk '{print $2}' > userlist.ldap`.
Final list of five accounts after removing un-needed accounts from above file.
```bash
cat userlist.ldap
sebastien
lucinda
andy
mark
santi
```
