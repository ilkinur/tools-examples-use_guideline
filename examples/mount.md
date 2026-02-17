# mount 

`showmount -e $IP`

`mount -t nfs $IP:/mnt/share /mnt/new -o nolock`

`showmount -e ip`   - Using showmount to display the mounted file share.

`mount -t nfs ip:/users ./mnt/` - ip and users is shared folder

---------

imageleri fayl sistem kimi acmaq ucun  
`sudo mkdir /mnt/image`  
`sudo mount -o loop disk.img /mnt/image`  

sonda umount et  
`sudo umount /mnt/image`
