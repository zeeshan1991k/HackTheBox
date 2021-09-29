# Privilege Escalation to Root
## Using `sudo -l` reveals that command ` /usr/bin/python3 /opt/remote-manage.py` can run as sudo 
```bash
user@forge:~$ sudo -l
Matching Defaults entries for user on forge:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User user may run the following commands on forge:
    (ALL : ALL) NOPASSWD: /usr/bin/python3 /opt/remote-manage.py
```
## remote-manage.py  code analysis
```python
#!/usr/bin/env python3
import socket
import random
import subprocess
import pdb

port = random.randint(1025, 65535)

try:
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    sock.bind(('127.0.0.1', port))
    sock.listen(1)
    print(f'Listening on localhost:{port}')
    (clientsock, addr) = sock.accept()
    clientsock.send(b'Enter the secret passsword: ')
    if clientsock.recv(1024).strip().decode() != 'secretadminpassword':
        clientsock.send(b'Wrong password!\n')
    else:
        clientsock.send(b'Welcome admin!\n')
        while True:
            clientsock.send(b'\nWhat do you wanna do: \n')
            clientsock.send(b'[1] View processes\n')
            clientsock.send(b'[2] View free memory\n')
            clientsock.send(b'[3] View listening sockets\n')
            clientsock.send(b'[4] Quit\n')
            option = int(clientsock.recv(1024).strip())
            if option == 1:
                clientsock.send(subprocess.getoutput('ps aux').encode())
            elif option == 2:
                clientsock.send(subprocess.getoutput('df').encode())
            elif option == 3:
                clientsock.send(subprocess.getoutput('ss -lnt').encode())
            elif option == 4:
                clientsock.send(b'Bye\n')
                break
except Exception as e:
    print(e)
    pdb.post_mortem(e.__traceback__)
finally:
    quit()
```
When this python program is run , it is listening at localhost on any random port from `1025-65535`, when someone connects to this address `localhost` with port `1025-65535` via `nc localhost [port]`, it asks for password which should be `secretadminpassword`, if password is wrong it gives message `Wrong password!`, if password is correct then it gives message `What do you wanna do:` and gives 4 options asking `What do you wanna do:` ,if we press `1-3` , it will `View processes`,
`View free memory`,`View listening sockets` respectively and if `4` is pressed , then program will close . On the other hand , if we give any string instead of `1-4` options then it will `python debugger`, in which execute python commands to get shell 
as root like `os.system("/bin/bash")`.
## Getting shell as root
![[Pasted image 20210929214834.png]]

