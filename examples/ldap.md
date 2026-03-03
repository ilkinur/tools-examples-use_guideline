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


