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
![[Pasted image 20210728212541.png]]

