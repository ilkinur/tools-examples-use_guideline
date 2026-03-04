# NTP

NTP is designed to synchronize clocks of networked computers. It uses UDP port 123 as its
primary means of communication. 

The following are some pieces of information an attacker can obtain by querying an NTP server:
- List of hosts connected to the NTP server
- Clients IP addresses in the network, their system names, and OSs
- Internal IPs, if the NTP server is in the demilitarized zone (DMZ)


### ntpdate

This command collects the number of time samples from several time sources. Its syntax
is as follows:  
`ntpdate [-46bBdqsuv] [-a key] [-e authdelay] [-k keyfile] [-oversion] [-p samples] [-t timeout] [ -U user_name] server [...]` 


### ntptrace

This command determines where the NTP server obtains the time from and follows the
chain of NTP servers back to its primary time source. Attackers use this command to trace
the list of NTP servers connected to the network. Its syntax is as follows:  
`ntptrace [-n] [-m maxhosts] [servername/IP_address]`

### ntpdc

This command queries the ntpd daemon regarding its current state and requests changes
in that state. Attackers use this command to retrieve the state and statistics of each NTP
server connected to the target network. Its syntax is as follows:  
`ntpde [ -46dilnps ] [ -c command] [hostname/IP_address]` 

### ntpq

This command monitors the operations of the NTP daemon ntpd_ and determines its
performance. Its syntax is as follows: 
`ntpq [-46dinp] [-c command] [host/IP_address]`

