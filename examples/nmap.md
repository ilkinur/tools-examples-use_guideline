# NMAP

 ### nmap - Enum Users
 
 `nmap -p 445 --script smb-enum-users \<target\> --script-args smbuser=username,smbpass=password,smbdomain=domain`  
 
 `nmap -p 445 --script smb-enum-users \<target\> --script-args smbuser=username,smbhash=LM:NTLM,smbdomain=domain`
   
`nmap --script smb-enum-users.nse --script-args smbusername=User1,smbpass=Pass@1234,smbdomain=workstation -p445 192.168.1.10`
   
`nmap --script smb-enum-users.nse --script-args smbusername=User1,smbhash=aad3b435b51404eeaad3b435b51404ee:C318D62C8B3CA508DD753DDA8CC74028,smbdomain=mydomain -p445 192.168.1.10<br>`
 
 ### nmap - Enum Groups
 
 `nmap -p 445 --script smb-enum-groups \<target\> --script-args smbuser=username,smbpass=password,smbdomain=domain`  
 
 `nmap -p 445 --script smb-enum-groups \<target\> --script-args smbuser=username,smbhash=LM:NTLM,smbdomain=domain`
 
 ### nmap - Enum Shares
 
 `nmap -p 445 --script smb-enum-shares \<target\> --script-args smbuser=username,smbpass=password,smbdomain=domain`  
 
 `nmap -p 445 --script smb-enum-shares \<target\> --script-args smbuser=username,smbpass=LM:NTLM,smbdomain=domain`
 
 ### nmap - OS Discovery
 
 `nmap -p 445 --script smb-os-discovery \<target\>`
 
 ### nmap - SMB Vulnerabilities on Windows
 
 `nmap -p 445 --script smb-vuln-ms06-025 target-IP <br>`  
 
 `nmap -p 445 --script smb-vuln-ms07-029 target-IP <br>`  
 
 `nmap -p 445 --script smb-vuln-ms08-067 target-IP <br>`  
 
 `nmap -p 445 --script smb-vuln-ms10-054 target-IP <br>`  
 
 `nmap -p 445 --script smb-vuln-ms10-061 target-IP <br>`  
 
 `nmap -p 445 --script smb-vuln-ms17-010 target-IP <br>`  
 
 `nmap -p 445 --script smb-vuln-cve-2017-7494 target-IP <br>`  
 
 -- Always check for updated list on `https://nmap.org/nsedoc/scripts/`
 
 ### map - Brute Force Accounts (be aware of account lockout!)  
 
 `nmap вЂ“p 445 --script smb-brute вЂ“script-args userdb=user-list.txt,passdb=pass-list.txt target-IP`

 


 
