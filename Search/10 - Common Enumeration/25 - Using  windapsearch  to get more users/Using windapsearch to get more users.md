# 25 - Using windapsearch to get more users
```bash
~/Dropbox/Documents/htb/boxes/search ❯ windapsearch -d 'search.htb' -u 'hope.sharp' -p 'IsolationIsKey?' -m users | grep -i "sAMAccountName" | awk '{print $2}'
Payton.Harmon
Trace.Ryan
krbtgt
Santino.Benjamin
Administrator
Guest
Reginald.Morton
Savanah.Velazquez
Antony.Russo
Eddie.Stevens
Cortez.Hickman
Chace.Oneill
Abril.Suarez
Cameron.Melendez
Blaine.Zavala
Bobby.Wolf
Arielle.Schultz
Lane.Wu
Edith.Walls
Margaret.Robinson
Sarai.Boone
Celia.Moreno
Kaitlynn.Lee
Jermaine.Franco
Alfred.Chan
Jamar.Holt
Kyler.Arias
Saniyah.Roy
Yareli.Mcintyre
Griffin.Maddox
Sandra.Wolfe
Rene.Larson
Prince.Hobbs
Armando.Nash
Savanah.Knox
Amare.Serrano
Sonia.Schneider
Maeve.Mann
Lizeth.Love
Frederick.Cuevas
Marshall.Skinner
Edgar.Jacobs
Elisha.Watts
Belen.Compton
Maren.Guzman
Amari.Mora
Natasha.Mayer
Sage.Henson
Katelynn.Costa
Cadence.Conner
Chanel.Bell
Scarlett.Parks
Eliezer.Jordan
Dax.Santiago
Lillie.Saunders
Jayla.Roberts
Lorelei.Huang
Claudia.Sharp
Abbigail.Turner
Taniya.Hardy
Charlee.Wilkinson
Monique.Moreno
Desmond.Bonilla
Yaritza.Riddle
Zain.Hopkins
Tori.Mora
Hugo.Forbes
Jolie.Lee
Hope.Sharp
Melanie.Santiago
Hunter.Kirby
German.Rice
Kylee.Davila
Gunnar.Callahan
Aarav.Fry
Annabelle.Wells
Jeramiah.Fritz
Cade.Austin
Ada.Gillespie
Colby.Russell
Eve.Galvan
Keely.Lyons
Abby.Gonzalez
Joy.Costa
Vincent.Sutton
Cesar.Yang
Camren.Luna
Tyshawn.Peck
Braeden.Rasmussen
Judah.Frye
Keith.Hester
Maci.Graves
Angel.Atkinson
Sierra.Frye
Tristen.Christian
Crystal.Greer
Kayley.Ferguson
Haven.Summers
Isabela.Estrada
web_svc
Kaylin.Bird
Angie.Duffy
Tristan.Davies
Claudia.Pugh
Jordan.Gregory
```
Found web_svc account , can be used to get service ticket(TGS) as it is registered with SPN(Service Principal Name) as shown below.
```bash
~/Dropbox/Documents/htb/boxes/search ❯ windapsearch -d 'search.htb' -u 'hope.sharp' -p 'IsolationIsKey?' -m user-spns
dn: CN=Web Service,OU=Users,OU=Sheffield,OU=Sites,DC=search,DC=htb
cn: Web Service
sAMAccountName: web_svc
servicePrincipalName: RESEARCH/web_svc.search.htb:60001
```

