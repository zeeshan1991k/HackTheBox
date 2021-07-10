# Common Enumeration
## NMAP
All ports scan without service version and os detection.
```bash
❯ sudo nmap -p- --min-rate 10000 -oA nmap/heist 10.10.10.149
[sudo] password for kali:
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-11 00:01 PKT
Nmap scan report for 10.10.10.149
Host is up (0.18s latency).
Not shown: 65531 filtered ports
PORT     STATE SERVICE
80/tcp   open  http
135/tcp  open  msrpc
445/tcp  open  microsoft-ds
5985/tcp open  wsman

Nmap done: 1 IP address (1 host up) scanned in 14.85 seconds
```

