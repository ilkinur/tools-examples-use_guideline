# Active Directory enumeration

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



