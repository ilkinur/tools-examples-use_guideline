# SMBClient

 ### List shares on a machine using NULL Session
 
 `smbclient -L <target-IP>`
 
 ### List shares on a machine using a valid username + password
 
`smbclient -L \<target-IP\> -U username%password`
 
 ### Connect to a valid share with username + password
 
`smbclient //\<target\>/\<share$\> -U username%password`
  
 ### List files on a specific share
 
 `smbclient //\<target\>/\<share$\> -c 'ls' password -U username`
 
 ### List files on a specific share folder inside the share
 
 `smbclient //\<target\>/\<share$\> -c 'cd folder; ls' password -U username`
 
 ### Download a file from a specific share folder
 
 `smbclient //\<target\>/\<share$\> -c 'cd folder;get desired_file_name' password -U username`
  
 ### Copy a file to a specific share folder
 
`smbclient //\<target\>/\<share$\> -c 'put /var/www/my_local_file.txt .\target_folder\target_file.txt' password -U username`
 
 ### Create a folder in a specific share folder
 
`smbclient //\<target\>/\<share$\> -c 'mkdir .\target_folder\new_folder' password -U username`
 
 ### Rename a file in a specific share folder
 
`smbclient //\<target\>/\<share$\> -c 'rename current_file.txt new_file.txt' password -U username`
 
 ---
`smbclient -L \\<RHOST>\ -N`  
`smbclient -L //<RHOST>/ -N`  
`smbclient -L ////<RHOST>/ -N`  
`smbclient -U "<USERNAME>" -L \\\\<RHOST>\\`  
`smbclient -L //<RHOST>// -U <USERNAME>%<PASSWORD>` 
`smbclient //<RHOST>/SYSVOL -U <USERNAME>%<PASSWORD>`  
`smbclient "\\\\<RHOST>\<SHARE>"`  
`smbclient \\\\<RHOST>\\<SHARE> -U '<USERNAME>' --socket-options='TCP_NODELAY IPTOS_LOWsmbclient --no-pass //<RHOST>/<SHARE>`  
`mount.cifs //<RHOST>/<SHARE> /mnt/remote`  
`guestmount --add '/<MOUNTPOINT>/<DIRECTORY/FILE>' --inspector --ro /mnt/<MOUNT> -v`  
 

Download multiple files at once  
```bash
mask""  
recurse ON  
prompt OFF  
mget *
```  

Upload multiple Files at once  
```bash
recurse ON  
prompt OFF  
mput *
```  

