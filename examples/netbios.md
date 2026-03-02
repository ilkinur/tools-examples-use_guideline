# NetBIOS


### NetBIOS enumeration

A NetBIOS name is a unique 16 ASCII character string used to identify the network devices over TCP/IP; fifteen characters are used for the device name, 
and the sixteenth character is reserved for the service or name record type 

Attackers use NetBIOS enumeration to obtain the following:
- The list of computers that belong to a domain
- The list of shares on the individual hosts in a network
- Policies and passwords

##### Nbtstat Utility

Nbtstat is a Windows utility that helps in troubleshooting NETBIOS name resolution problems.
The nbtstat command removes and corrects preloaded entries using several case-sensitive
switches. Attackers use Nbtstat to enumerate information such as NetBIOS over TCP/IP (NetBT)
protocol statistics, NetBIOS name tables for both local and remote computers, and the NetBIOS
name cache. 

`nbtstat [-a RemoteName] [-A IP Address] [-c] [-n] [-r] [-R] [-RR] [-s] [-S] [Interval]`

`nmap -sV -v --script nbstat.nse <target IP address>`

##### Enumerating Shared Resources Using Net View 

`net view \\<computername>`  
In the above command, <computername> is the name or IP address of a specific computer, the resources of which are to be displayed.  
`net view \\<computername> /ALL`  
The above command displays all the shares on the specified remote computer, along with hidden shares.  
`net view /domain`  
The above command displays all the shares in the domain.  
`net view /domain:<domain name>`  
The above command displays all the shares on the specified domain.  




