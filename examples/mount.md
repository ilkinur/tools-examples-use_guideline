showmount -e $IP

mount -t nfs $IP:/mnt/share /mnt/new -o nolock