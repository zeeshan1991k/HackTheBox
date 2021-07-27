# Enumeration using RPCCLIENT
`enum4linux` uses rpcclient to enumerate domain users so rpcclient can be used  individually to enumerate `domain users`.
![[Pasted image 20210727103059.png]]
We can determine `domain users` via command `rpcclient -U '' 10.10.10.161`
which needs NULL username to log in otherwise it will not log in.



