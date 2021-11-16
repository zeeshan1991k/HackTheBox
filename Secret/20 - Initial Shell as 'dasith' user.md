# Initial Shell as 'dasith' user
## Download source code of website(DUMBDocs)
![[Pasted image 20211116181701.png]]
## Unzipped contents of downloaded source code
![[Pasted image 20211116181958.png]]
## As source code was git repo , so `git log` showed that .env file was modified to hide TOKEN_SECRET
![[Pasted image 20211116182220.png]]
## So reverted to inital version of git repo that included TOKEN_SECRET
![[Pasted image 20211116182637.png]]
## `private.js` file in source code showed that if name included 'theadmin', then we can log in as 'admin' user
```javascript
const router = require('express').Router();
const verifytoken = require('./verifytoken')
const User = require('../model/user');

router.get('/priv', verifytoken, (req, res) => {
   // res.send(req.user)

    const userinfo = { name: req.user }

    const name = userinfo.name.name;

    if (name == 'theadmin'){
        res.json({
            role:{

                role:"you are admin",
                desc : "{flag will be here}"
            }
        })
    }
    else{
        res.json({
            role: {
                role: "you are normal user",
                desc: userinfo.name.name
            }
        })
    }

})

router.use(function (req, res, next) {
    res.json({
        message: {

            message: "404 page not found",
            desc: "page you are looking for is not found. "
        }
    })
});


module.exports = router
```
![[Pasted image 20211116182933.png]]
## TOKEN_SECRET can be used to generate JWT web-token(`auth-token :`) which can be used to login as 'admin'
![[Pasted image 20211116215200.png]]
## Creating jwt web-token to login as `admin`
![[Pasted image 20211116222014.png]]
## 