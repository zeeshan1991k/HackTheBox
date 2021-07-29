#  Inspecting 10.10.10.228 webpage 'check book' button
## Searching and clicking on book button
Searching and clicking on book button pops up following page.
 ![[Pasted image 20210729152643.png]]
 Its request in burpsuite shows that it is requesting html file `book7.html` from `/includes/bookController.php`so there is potential for  Local File inclusion attack 
![[Pasted image 20210729153020.png]]
Trying file on server that does not exist gives error in response that file is not present on server and also discloses the whole path.
![[Pasted image 20210729153748.png]]
So we can do Local File Inclusion example of which is given in screenshot below.
![[Pasted image 20210729154539.png]]

 