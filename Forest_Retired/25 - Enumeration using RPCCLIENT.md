# Enumeration using RPCCLIENT
`enum4linux` uses rpcclient to enumerate domain users so rpcclient can be used  individually to enumerate `domain users`.
![[Pasted image 20210727103059.png]]
We can determine `domain users` via command ` rpcclient -U '' -N 10.10.10.161`
which needs NULL username to log in otherwise it will not log in as anonymous user. So after getting rpcclient prompt , we can use command `enumdomusers` to get domain users.
```bash
❯ rpcclient -U '' -N 10.10.10.161
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[DefaultAccount] rid:[0x1f7]
user:[$331000-VK4ADACQNUCA] rid:[0x463]
user:[SM_2c8eef0a09b545acb] rid:[0x464]
user:[SM_ca8c2ed5bdab4dc9b] rid:[0x465]
user:[SM_75a538d3025e4db9a] rid:[0x466]
user:[SM_681f53d4942840e18] rid:[0x467]
user:[SM_1b41c9286325456bb] rid:[0x468]
user:[SM_9b69f1b9d2cc45549] rid:[0x469]
user:[SM_7c96b981967141ebb] rid:[0x46a]
user:[SM_c75ee099d0a64c91b] rid:[0x46b]
user:[SM_1ffab36a2f5f479cb] rid:[0x46c]
user:[HealthMailboxc3d7722] rid:[0x46e]
user:[HealthMailboxfc9daad] rid:[0x46f]
user:[HealthMailboxc0a90c9] rid:[0x470]
user:[HealthMailbox670628e] rid:[0x471]
user:[HealthMailbox968e74d] rid:[0x472]
user:[HealthMailbox6ded678] rid:[0x473]
user:[HealthMailbox83d6781] rid:[0x474]
user:[HealthMailboxfd87238] rid:[0x475]
user:[HealthMailboxb01ac64] rid:[0x476]
user:[HealthMailbox7108a4e] rid:[0x477]
user:[HealthMailbox0659cc1] rid:[0x478]
user:[sebastien] rid:[0x479]
user:[lucinda] rid:[0x47a]
user:[svc-alfresco] rid:[0x47b]
user:[andy] rid:[0x47e]
user:[mark] rid:[0x47f]
user:[santi] rid:[0x480]
```
`user:[svc-alfresco] rid:[0x47b]` user is new . so it can be added it to our users' list. 

Using command `queryusergroups 0x47b(rid of user svc-alfresco)` , it is known that `svc-alfresco` user belongs to two groups.
```bash
rpcclient $> queryusergroups 0x47b
        group rid:[0x201] attr:[0x7]
        group rid:[0x47c] attr:[0x7]
rpcclient $>
```
We can query the groups in which user `svc-alfresco` is in via following commands.
```bash
rpcclient $> querygroup 0x201
        Group Name:     Domain Users
        Description:    All domain users
        Group Attribute:7
        Num Members:30
rpcclient $> querygroup 0x47c
        Group Name:     Service Accounts
        Description:
        Group Attribute:7
        Num Members:1
rpcclient $>
```
We can also query specific user like `svc-alfresco` by giving its rid with `queryuser`.
```bash
rpcclient $> queryuser 0x47b(rid of svc-alfresco user)
        User Name   :   svc-alfresco
        Full Name   :   svc-alfresco
        Home Drive  :
        Dir Drive   :
        Profile Path:
        Logon Script:
        Description :
        Workstations:
        Comment     :
        Remote Dial :
        Logon Time               :      Mon, 23 Sep 2019 16:09:48 PKT
        Logoff Time              :      Thu, 01 Jan 1970 05:00:00 +05
        Kickoff Time             :      Thu, 01 Jan 1970 05:00:00 +05
        Password last set Time   :      Tue, 27 Jul 2021 13:08:27 PKT
        Password can change Time :      Wed, 28 Jul 2021 13:08:27 PKT
        Password must change Time:      Thu, 14 Sep 30828 07:48:05 PKT
        unknown_2[0..31]...
        user_rid :      0x47b
        group_rid:      0x201
        acb_info :      0x00010210
        fields_present: 0x00ffffff
        logon_divs:     168
        bad_password_count:     0x00000000
        logon_count:    0x00000006
        padding1[0..7]...
        logon_hrs[0..21]...
rpcclient $>
```




