# Generating a worldlist for Password Spraying
## Writing a general password list
```bash
January
February
March
April
May
June
July
August
September
October
November
December
Password
P@ssw0rd
Forest
htb
Secret
Autumn
Fall
Spring
Winter
Summer
```
## Generating password(above) list with years(2019,2020) appended at the end
```bash
for i in $(cat pwlist.txt); do echo $i; echo ${i}2019; echo ${i}2020; done
January
January2019
January2020
February
February2019
February2020
March
March2019
March2020
April
April2019
April2020
May
May2019
May2020
June
June2019
June2020
July
July2019
July2020
August
August2019
August2020
September
September2019
September2020
October
October2019
October2020
November
November2019
November2020
December
December2019
December2020
Password
Password2019
Password2020
P@ssw0rd
P@ssw0rd2019
P@ssw0rd2020
Forest
Forest2019
Forest2020
htb
htb2019
htb2020
Secret
Secret2019
Secret2020
Autumn
Autumn2019
Autumn2020
Fall
Fall2019
Fall2020
Spring
Spring2019
Spring2020
Winter
Winter2019
Winter2020
Summer
Summer2019
Summer2020
```
Note: In`cat t > t` , file `t` will be empty , so be careful.
# Using Hashcat to make different variations in above wordlist
`hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule`
does not have exclamation point, so adding exclamation point via this command `for i in $(cat pwlist.txt); do echo $i; echo ${i}\!; done > t`. We can also chain wordlists so we can use `hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule` which toggle various uppercases. But this is going to have alot of duplicates, so using `sort -u` to exclude duplicates via command ` hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | sort -u`.  These will be alot of passwords, so only including passwords of characters greater than `7` via this command `hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | sort -u | awk 'length($0) > 7'`.


