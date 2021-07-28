# Portal login page
## Making new account on portal login page
Making new account on portal login page with user `clarkey` and password `1234` and logged in with `clarkey` user.
![[Pasted image 20210728193638.png]]
Awaiting approval in above screenshot means that we have to do XSS to get admin approval for our newly created account `clarkey`.
## Checking `check tasks` button
This button brings us to following page.
![[Pasted image 20210728210258.png]]
`# Fix PHPSESSID infinite session duration` means that there is some type of issue with how they are generating php sessions and cookie itself does not have a timout thing.
# Checking `PHPSESSID` in developer tools of mozilla
Checking `PHPSESSID` in developer tools of mozilla, it is found that `PHPSESSID` consists of username plus md5 hash.. 
![[Pasted image 20210728212541.png]]
As md5 hash has 32 characters so it confirms that portion after username in `PHPSESSID` is md5 hash.
```bash
❯ echo -n 403ad0fb70435e8159bd13e237210369 | wc -c
32
```
