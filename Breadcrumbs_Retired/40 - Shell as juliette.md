# Shell as juliette
## Found New `pizzaDeliveryUserData` directory in portal directory
Found New `pizzaDeliveryUserData` directory in portal directory, which contains some files of which `juliette.json` file gave some creds.
```powershell
cd pizzaDeliveryUserData
cd pizzaDeliveryUserData
ls
ls


    Directory: C:\Users\www-data\Desktop\xampp\htdocs\portal\pizzaDeliveryUserData


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----        11/28/2020   1:48 AM            170 alex.disabled
-a----        11/28/2020   1:48 AM            170 emma.disabled
-a----        11/28/2020   1:48 AM            170 jack.disabled
-a----        11/28/2020   1:48 AM            170 john.disabled
-a----         1/17/2021   3:11 PM            192 juliette.json
-a----        11/28/2020   1:48 AM            170 lucas.disabled
-a----        11/28/2020   1:48 AM            170 olivia.disabled
-a----        11/28/2020   1:48 AM            170 paul.disabled
-a----        11/28/2020   1:48 AM            170 sirine.disabled
-a----        11/28/2020   1:48 AM            170 william.disabled


cat juliette.json
cat juliette.json
{
        "pizza" : "margherita",
        "size" : "large",
        "drink" : "water",
        "card" : "VISA",
        "PIN" : "9890",
        "alternate" : {
                "username" : "juliette",
                "password" : "jUli901./())!",
        }
}
PS C:\Users\www-data\Desktop\xampp\htdocs\portal\pizzaDeliveryUserData>
```
## SSH logged in as juliette user using juliette creds
![[Pasted image 20210730122829.png]]

