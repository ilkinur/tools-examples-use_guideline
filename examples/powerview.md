# PowerView


```powershell
import-module .\powerview.ps1
Get-DomainGroup -MemberIdentity "username"
```
Show us usergroup information. The important parts are samaccountname, memberof, member



`$pass = ConvertTo-SecureString 'test' -AsPlainText -Force`
