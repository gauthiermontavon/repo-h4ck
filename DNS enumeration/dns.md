## DNS footprinting

### using nslookup
```
nslookup
>set type=A
>www.website.com
server:
address:

Non-authorative answer...
...

>set type=cname
>website.com



### windows
nslookup
Resolve-DnsName

### unix
dig 


host 


## online tools
- dns lookup
- mxtoolbox
- dnswatch
- dnsdumpster

## DNS zone transfert
### using dig
```
dig ns <target domain>
dig @<domain of name server> <target domain> axfr //TRANSFER
```
### using nslookup
```
nslookup 
set querytype=soa 
<target domain>
/ls -d <domain of name server	// TRANSFER
```
### using dnsRecon
```
dnsrecon -t axfr -d <target domain>
```

## DNS Cache Snooping

DNS enum technique whereby attacker queries the DNS server for a specific cached DNS record.

non-recursive method : if no present, dns response with info about another DNS.
```
dig @<IP of DNS server> <Target domain> A +norecurse
```
recursive method: if no present, add to cache.
```
dig @<IP of DNS server> <Target domain> A +recurse
```
## DNSSEC Zone walking
DNS enum technique where an attacker attempts to obtain internal records of the DNS server if the DNS zone is not properly configured
```

ldns-walk @<IP of DNS Server> <Target domain>
```
```
dnsrecon -d <target domain> -z
```
Others tools :
- LDNS
- DNSRecon
- nsec3map
- nsec3walker
- DNSwalk


## DNS Enum using OWASP Amass
(A docker image for amass)
```
amass enum -passive -d <Target Domain> -src
```
```
amass enum -active -d <Target Domain> -brute -w /usr/share/wordlists/amass/all.txt
```
```
amass track -config /root/amass/config.ini -dir amass4owasp -d <Target Domain> -last 2
```
```
amass db -dir amass4owasp -list
```
```
amass viz -d3 -dir amass4owasp
```
## DNS and DNSSEC enum using nmap
```
nmap --script=broadcast-dns-service-discovery <Target Domain>
```
```
nmap -T4 -p 53 --script dns-brute <Target Domain>
```
```
nmap -Pn -sU -p 53 --script=dns-recursion 192.168.1.150 //check recursion
```
```
nmap -sU -p 53 --script dns-nsec-enum --script-args dns-nsec-enum.domains= eccouncil.org <target>
```
