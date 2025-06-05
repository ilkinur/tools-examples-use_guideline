# PowerShell

```powershell
whoami /all
getuserid
systeminfo
Get-Process
net users
net users <USERNAME>
Get-ADUser -Filter * -SearchBase "DC=<DOMAIN>,DC=LOCAL"
Get-Content <FILE>
Get-ChildItem . -Force
GCI -hidden
type <FILE> | findstr /l <STRING>
[convert]::ToBase64String((Get-Content -path "<FILE>" -Encoding byte))
```

## Allow Script Execution 
`Set-ExecutionPolicy remotesigned`  
`Set-ExecutionPolicy unrestricted`  

## Script Execution Bypass
`powershell.exe -noprofile -executionpolicy bypass -file .\<FILE>.ps1`

