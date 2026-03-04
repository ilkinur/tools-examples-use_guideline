# NFS

NFS is a type of file system that enables users to access, view, store, and update files over a
remote server. 

`/etc/exports` - location on the NFS server contains a list of clients
allowed to share files on the server. 

`rpcinfo` command to scan the target
IP address for an open NFS port (port 2049) and the NFS services running on it:  
`rpcinfo -p <Target IP Address>`

runs the following command to view the list of shared
files and directories:  
`showmount -e <Target IP Address>`
