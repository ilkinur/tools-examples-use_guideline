# DNS

### dig
the following command to perform DNS
zone transfer:  
`dig ns <target domain>`  

one of the name servers from the output of the above command to test
whether the target DNS allows zone transfers.  
`dig @<domain of name server> <target domain> axfr`

### nslookup

```bash
nslookup
set querytype=soa
<target domain> 
```

### DNSRecon

`dnsrecon -t axfr -d <target domain> `


## DNS Cache Snooping 

DNS cache snoopingis a DNS enumeration technique wherebyan attacker queries the DNS server for a specific cached DNS record 

##### Non-recursive Method 

Attackers send a non-recursive query by | setting the Recursion Desired (RD) bit in the query header to zero  
`dig @<IP of DNS server> <Target domain> A +norecurse`

##### Recursive Method 

Attackers send a recursive query to determine the time the DNS record resides in the cache  
`dig @<IP of DNS server> <Target domain> A +recurse`


## DNSSEC Zone Walking 

DNSSEC zone walkingis a DNS enumeration technique where an attacker attempts to obtain internal records of the DNS server if the
DNS zone is not properly configured 

`ldns-walk @<IP of DNS Server> <Target domain>`

`dnsrecon -d <target domain> -z`


```bash
nmap --script=broadcast-dns-service-discovery <Target Domain>
nmap -T4 -p 53 --script dns-brute <Target Domain>
nmap -Pn -sU -p 53 --script=dns-recursion 192.168.1.150
nmap -sU -p 53 --script dns-nsec-enum --script-args dns-nsec-enum.domains=eccouncil.org <target> 
```
