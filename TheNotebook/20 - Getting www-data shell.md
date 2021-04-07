# Getting www-data shell
## Making account on http://10.10.10.230/register
![[Pasted image 20210405080637.png]]
## Logging in as 'clarkey' user
![[Pasted image 20210405080821.png]]
Logged in as 'clarkey user'.
![[Pasted image 20210405080908.png]]
## Fetching JSON Web-token via burp
![[Pasted image 20210405081234.png]]
## Decoding JSON Web-token on https://jwt.io/
![[Pasted image 20210405081847.png]]
## Generating private and public key on local machine 
Generating private and public key on local machine for signing JSON Web Token.
```bash
❯ ./jwtRS256.sh
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in jwtRS256.key
Your public key has been saved in jwtRS256.key.pub
The key fingerprint is:
SHA256:2C5sfymMLQXejJsv1EHgq52v+UvABTftX3N8rLTI5J4 kali@kali
The key's randomart image is:
+---[RSA 4096]----+
|      o.o.       |
|     . o...      |
|      ....     o |
|     ..=. . . + =|
|     .=*S. = + =.|
|     +++=   = o  |
|    ..*Bo  o .   |
|     .**+ o E    |
|      o**+       |
+----[SHA256]-----+
writing RSA key
-----BEGIN RSA PRIVATE KEY-----
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
-----END RSA PRIVATE KEY-----
-----BEGIN PUBLIC KEY-----
MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICCgKCAgEA3+yR6lRP7hVYtOIid5HB
H7tK4LPVjf7p+QCfwThJYUHm9z7CazQf+i2zH+X+F5OxgrQC9u1scZdz01BuLOGR
PtQnTvfpRleXy7e860tqcxh/uGKj+KZARiN6EG7aEgsxjqWskYcFaLqh57BlGBue
ddfmT3Rnj6Z03IwmuzFZSS7VCg9dM3jT6WSObM+wE+fzSM+vSIUyFXKw8mzQXYqt
FTfdA0mLgJEuC2mJXlrQni9oBNTJc8182K4JBOLLs9mUgbsqrTVUse2HOsEbJ6ir
bhZzan8rjpd05DDwTxAzBgzfzGdNQffAe9imM+Bxh8sw6yTphGcBixjG4kIAeXea
tmwpghzq439Y2twCOqz8JBVAR0qSi244IsOYjxHqiKBKTpX3UOIvWkHhmQDNJIyr
rmuSJhbM8LVxehmPwyiuSLf3UG6IYXHuriUp7anNuaKDv38jCzv9n0L5K2KG3yHZ
+jCMwCSaqIAIMp4thjdezuo4vMlddCyKf7nSARGmB8KD4/sb2IS/SRFUEeFDJu6F
EV7Qe657UthM1M3RHNjUsKWQ5rPDG+qLzlgD3REEf/eWVl70XHYfVXKnREqQlSlh
U3d6yVH5gb3T0sSXT4dm0VV5weLBxkkTTeBdDc0zRWOp8EJrPemrao9u0lZVuD7a
qv5YsS0JZFji1PtaJbLKA4sCAwEAAQ==
-----END PUBLIC KEY-----
```

## Changing JSON Web Token to log in as admin
![[Pasted image 20210405090237.png]]
## Changed JSON Web Token in firefox plugin Cookie editor
![[Pasted image 20210407101513.png]]
## Logged in as admin
![[Pasted image 20210405091027.png]]
## Got reverse shell as www-data
![[Pasted image 20210405091704.png]]

