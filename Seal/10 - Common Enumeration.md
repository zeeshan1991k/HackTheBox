# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
 ❯ sudo nmap -p- --min-rate 10000 -oA nmap/seal 10.10.10.250
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-22 17:27 PKT
Warning: 10.10.10.250 giving up on port because retransmission cap hit (10).
Nmap scan report for 10.10.10.250
Host is up (0.10s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
443/tcp  open  https
8080/tcp open  http-proxy

Nmap done: 1 IP address (1 host up) scanned in 13.49 seconds
```
Detected ports scan with service version and os detection.
```bash

```