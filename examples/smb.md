# SMB examples

`smbclient -L 10.10.10.10 -N` - list with anonymous (-N)

`smbclient \\\\10.10.10.10\\{anything}` - open directory



get all files
```bash
smbclient '\\server\share'
mask ""
recurse ON
prompt OFF
cd 'path\to\remote\dir'
lcd '~/path/to/download/to/'
mget *
```
