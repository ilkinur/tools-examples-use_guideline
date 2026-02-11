# Kerbrute

#### Overview
- Kerbrute is a Kerberos-focused Active Directory enumeration tool
- It is written in Go
- It interacts directly with the Kerberos protocol
- It does not require SMB, LDAP, or Windows APIs
- It is very fast and lightweight

#### Purpose
- Enumerate valid domain usernames
- Perform password spraying in lab environments
- Identify accounts vulnerable to Kerberos-based attacks
- Validate credentials without logging into systems

#### Why Kerbrute Is Important
- Kerberos responds differently to valid and invalid users
- Kerbrute abuses this behavior safely and efficiently
- No authentication is required for user enumeration
- Very stealthy compared to SMB or LDAP enumeration

####  How Kerbrute Works
- Sends Kerberos authentication requests to the Domain Controller
- Observes Kerberos error messages
- Distinguishes between:
- Valid users
- Invalid users
- Locked or disabled accounts
- Does not create Windows logon events


### Kerbrute Modes


#### User Enumeration Mode
- Identifies valid Active Directory usernames
- Does not require credentials
- Uses Kerberos preauthentication responses

Example:  
`kerbrute userenum --dc 192.168.1.10 users.txt domain.local`  

Input:
- Domain Controller IP
- Username wordlist
- Domain name

Output:
- List of valid domain users

#### Password Spraying Mode (Lab Only)
- Tests one password against many users
- Reduces account lockout risk
- Common in real-world misconfigurations

Example:  
`kerbrute passwordspray --dc 192.168.1.10 users.txt Password123 domain.local`  

Output:
- Valid username and password combinations


#### Credential Validation Mode
- Verifies known credentials
- Confirms whether a username and password are correct

Example:  
`kerbrute login --dc 192.168.1.10 user Password123 domain.local`



### User Enumeration  
`kerbrute userenum -d <DOMAIN> --dc <DOMAIN> /PATH/TO/FILE/<USERNAMES>`

### Password Spray  
`kerbrute passwordspray -d <DOMAIN> --dc <DOMAIN> /PATH/TO/FILE/<USERNAMES> <PASSWORD>`

