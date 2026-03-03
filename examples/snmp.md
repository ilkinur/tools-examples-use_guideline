# SNMP

Simple Network Management Protocol (SNMP) allows network administrators to manage
network devices from a remote location.

### SnmpWalk
SnmpWalk is a command-line tool that allows attackers to scan numerous Simple Network
Management Protocol (SNMP) nodes instantly and identify a set of variables that are available
for accessing the target network. 


`snmpwalk -vl -c public <Target IP Address>`  
The above command allows attackers to view all the OIDs, variables, and other associated
information.

- Command to enumerate SNMPv2 with a community string of public:  
`snmpwalk -v2c -c public <Target IP Address>`
- Command to search for installed software:  
`snmpwalk -v2c -c public <Target IP Address> hrSWInstalledName`
- Command to determine the amount of RAM on the host:  
`snmpwalk -v2c -c public <Target IP Address> hrMemorySize`
- Command to change an OID to a different value:  
`snmpwalk -v2c -c public <Target IP Address> <OID> <New Value>`
- Command to change the sysContact OID:  
`snmpwalk -v2c -c public <Target IP Address> sysContact <New Value> `

### Nmap

`nmap -sU -p 161 --script=snmp-processes <Target IP Address>`  

Other Nmap commands to perform SNMP enumeration:  
- `nmap -sU -p 161 --script=snmp-sysdescr<Target IP Address>`  
=> Retrieves information regarding SNMP server type and operating system details.  
- `nmap -sU -p 161 --script=snmp-win32-software <Target IP Address>`  
=> Retrieves a list of all the applications running on the target machine. 

### snmp-check 
snmp-check is an open-source tool distributed under the GNU General Public License
(GPL). Its goal is to automate the process of gathering information on any device with
SNMP support (Windows, Unix-like, network appliances, printers, etc.). snmp-check
allows the enumeration of SNMP devices and places the output in a human-readable and
user-friendly format. It could be useful for penetration testing or systems monitoring. 


---------------------------
```bash
snmpwalk -c public -v1 10.0.0.0
snmpcheck -t 192.168.1.X -c public
onesixtyone -c names -i hosts
nmap -sT -p 161 192.168.X.X -oG snmp_results.txt
snmpenum -t 192.168.1.X
```

