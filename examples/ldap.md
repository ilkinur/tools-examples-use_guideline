# LDAP

LDAP (Lightweight Directory Access Protocol) — şəbəkə üzərində directory (kataloq) xidmətlərinə qoşulmaq, məlumat oxumaq və idarə etmək üçün istifadə olunan tətbiq səviyyəli protokoldur.
LDAP əsasən:
- İstifadəçi autentifikasiyası
- Active Directory idarəetməsi
- Mərkəzləşdirilmiş user bazası
- Group və icazə idarəsi  
üçün istifadə olunur.

389  → LDAP  
636  → LDAPS (SSL/TLS)

### ldapsearch
the following command can be executed to obtain additional details related to the naming contexts:  
`ldapsearch -h <Target IP Address> -x -s base namingcontexts` 

For example, from the output of the above command, if the primary domain component
can be identified as Dc=,h DCt=lobcal , the following command can be used to obtain
more information about the primary domain:  
`ldapsearch -h <Target IP Address> -x -b "DC=htb,DC=local"`  

The following commands can be used to retrieve information about a specific object or
all the objects in a directory tree:  
`ldapsearch -h <Target IP Address> -x -b "DC=htb,DC=local"' (objectClass=Employee) '`  
=> retrieves information related to the object class Employee.  
`ldapsearch -x -h <Target IP Address> -b "DC=htb,DC=local" "objectclass=*"`  
=> retrieves information related to all the objects in the directory tree.  
The following command retrieves a list of users belonging to a particular object class:  
`ldapsearch -h <Target IP Address> -x -b "DC=htb,DC=local" '(objectClass= Employee) ' sAMAccountName sAMAccountType`  

`nmap -p 389 --script ldap-brute --script-args ldap.base='"cn=users ,dc=CEH,dc=com "' <Target IP Address>`  



`ldapsearch -H ldap://<ip> -s base -b "" namingContexts` - enumerate naming contexts

`ldapdomaindump -u '<domain>\<username>' -p '<password>' <ip>` - Domain dumping


---

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
