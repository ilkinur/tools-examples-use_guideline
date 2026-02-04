# Active Directory enumeration

### Rubeus (Windows only)

A powerful Windows-based tool designed explicitly for Kerberos-related security testing and enumeration. Rubeus automatically identifies vulnerable accounts and retrieves encrypted AS-REP hashes.  

Example Command: `Rubeus.exe asreproast`  

This command scans Active Directory, identifies accounts with pre-authentication disabled, and retrieves hashes ready for offline cracking.  

### Impacket’s GetNPUsers.py (Linux/Windows)  

Impacket provides a flexible Python script (GetNPUsers.py) to enumerate accounts in non-Windows environments. To test for the pre-authentication vulnerability, you must supply a users.txt file containing usernames.  

Example Command: `GetNPUsers.py tryhackme.loc/ -dc-ip 10.211.12.10 -usersfile users.txt -format hashcat -outputfile hashes.txt -no-pass`  

This command enumerates usernames listed in users.txt and collects AS-REP hashes for vulnerable accounts, saving them in hashes.txt for offline cracking.  

### Privileges

Let’s list some high privileges that can be pivotal in planning your next steps. The most interesting privileges to check for are:  

`SeImpersonatePrivilege`: As mentioned already, this privilege allows a process to impersonate the security context of another user after authentication. The “potato” attack revolves around abusing this privilege.  
`SeAssignPrimaryTokenPrivilege`: This privilege permits a process to assign the primary token of another user to a new process. It is used in conjunction with the SeImpersonatePrivilege privilege.  
`SeBackupPrivilege`: This privilege lets users read any file on the system, ignoring file permissions. Consequently, attackers can use it to dump sensitive files like the SAM or SYSTEM hive.  
`SeRestorePrivilege`: This privilege grants the ability to write to any file or registry key without adhering to the set file permissions. Hence, it can be abused to overwrite critical system files or registry settings.  
`SeDebugPrivilege`: This privilege allows the account to attach a debugger to any process. As a result, the attacker can use this privilege to dump memory from LSASS and extract credentials or even inject malicious code into privileged processes.  

### Discovered in environment variables

`Get-ChildItem Env:` or simply `dir env:`

### Enumerating Users and Groups with NET commands (CMD)

`net help`  
`net user /domain`  
`net user <username> /domain`  
`net group /domain`  
`net group <Group Name> / domain`  
`net localgroup`  
`net localgroup <Group Name>`  

<br>

`query user`, or `quser`  - to list users logged on to a machine  
`tasklist` - displays a list of currently running processes  
`wmic service get` - gather information about Windows services  `wmic service get Name,StartName` == `Get-WmiObject Win32_Service | select Name, StartName`  
In addition to `wmic`, we can enumerate services on a local machine using the `sc` command. (`sc query state= all`, `sc query state= all | find "Keyword"`, `sc qc <ServiceName>`)

<br>

`reg query "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUsername` - If saved, the password will be in plaintext.  
`HKLM\Security\Cache` - this one requires administrator access; moreover, the credentials will be hashed and require cracking.  
`reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` - get a list of installed applications.  
`reg query HKLM /f "password" /t REG_SZ /s` - to search the registry for password

<br>

### BloodHound
`bloodhound-python -u asrepuser1 -p qwerty123! -d tryhackme.loc -ns 10.211.12.10 -c All --zip`
Parameters explained:  
-u username: Specifies the username for authentication  
-p password: Specifies the password for authentication  
-d tryhackme.loc: Targets the specified domain for data collection  
-ns: Specifies the IP address of a DNS server  
-c All: Uses all available collection methods  
--zip: Compresses the output into a ZIP archive for easy import into BloodHound  



### LDEEP commands
This command shows the list of all computer accounts registered in the domain.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 computers`  

This command shows the configuration partition of Active Directory  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 conf`  

This command shows machines with delegation rights, including unsecure configurations like unconstrained delegation and resource-based constrained delegation (RBCD) relationships  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 delegations`  

This command shows domain-wide password and account lockout policy setings via LDAP.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 domain_policy`  

This command enumerates the Flexible Single Master Operations (FSMO) role holders within the domain.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 fsmo`  

This command shows the credentials and related details of Group Managed Service Accounts (gMSAs) from the domain.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 gmsa`  

This command shows the use of the ldeep tool to enumerate Group Policy Objects (GPOs) via LDAP in an Active Directory environment.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 gpo`  

This command shows the enumeration of Active Directory groups using the ldeep tool via LDAP.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 groups`  

This command shows the use of the ldeep tool to enumerate machine (computer) accounts in the Active Directory domain using LDAP.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 machines`  

This command shows the use of the ldeep tool to enumerate Organizational Units (OUs) in an Active Directory (AD) environment using LDAP.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 ou`  

This command shows the use of the ldeep tool to enumerate Active Directory Certificate Services ADCS) via LDAP.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 pkis`  

This command shows the AD schema atributes, which define the structure and rules for objects in the directory.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 schema`  

This command shows detailed information about the ESC3 certificate template.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 templates`  

This command lists all user objects found in the Active Directory domain.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 users`  

This command shows Kerberos pre-authentication  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 users nokrbpreauth`  

This command shows users with assigned SPNs (Service Principal Names).  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 users spn`  

This command successfully extracts a LAPS (Local Administrator Password Solution) password  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 laps`  

This command shows all groups (including nested) that the user (username) is a member of.  
`ldeep ldap -u username -p password -d domain.local -s ldap://192.168.1.20 memberships username -r` 

---

`Get-LocalGroupMember -Group Administrators` - Members of the following local groups  
`Get-NetLocalGroupMember -ComputerName ws01 -GroupName Administrators` - lokal Administrators qrupunun üzvlərini  
`Get-NetLoggedon -ComputerName ws04` - uzaq kompüterdə hazırda və ya yaxın zamanda login olmuş user-ləri göstərmək üçün istifadə olunur  
`PsLoggedOn64.exe \\ws04` - ws04 kompüterində kimlərin login olduğunu göstərmək üçündür (bu registeridə HKEY_USERS-ə baxaraq bu qərarı verir)  
`Get-NetSession -ComputerName ws04` - Bu komanda uzaq kompüterdə aktiv SMB (network) session-ları göstərir. (PowerView command [alternativ: `net session \\ws04`, `Get-SmbSession -CimSession ws04`)  
Üstdəki komanda zəif icazəli userlər tərəfindən çalışdırılanda birçox halda nəticə vermir bununda səbəbi registery-də o icazənin söndürülməsidir:  
`HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\SrvsvcSessionInfo`  





