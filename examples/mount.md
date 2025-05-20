`showmount -e $IP`

`mount -t nfs $IP:/mnt/share /mnt/new -o nolock`

`showmount -e ip`   - Using showmount to display the mounted file share.

`mount -t nfs ip:/users ./mnt/` - ip and users is shared folder

