## Active hosts on subnet
```
nmap -sn -PR 192.168.1.0/24
```
```
nmap -sn -PE 192.168.1.0/24
```
## Aggressive scan on host

```
nmap -p443,80,53,135,8080,8888 -A -O -sV -sC -T4 -oN nmapOutput 10.10.10.10
```

 ## OS Discovery
``` 
nmap -O <IP>
```
```
nmap –script smb-os-discovery.nse <target ip>
```
