find / -user root -perm -4000 -exec ls -ldb {} \;


find / -user root -perm /4000 2>/dev/null

find / \( -perm -u+s -or -perm -g+s  \) -type f -exec ls -l {} \; -- suid sqid bitleri aktiv olan fayllar listelenir

find / -user root -perm -4000 -exec ls -ldb {} \; 2>>/dev/null | grep "/bin" -- suid de root yetkisi olanlari gosterir

getcap -r / 2>/dev/null


find / -group GROUPNAME 2>/dev/null -  	Retrieve a list of files and directories owned by a specific group.

find / -perm -o+w 2>/dev/null  -   	Retrieve a list of all world-writable files and directories.

find / -type f -cmin -5 2>/dev/null  -  Retrieve a list of files created or changed within the last five minutes.

find / -type f -executable 2> /dev/null - on UNIX-based systems to discover all executable files within the filesystem quickly

find / -perm -u=s -type f 2>/dev/null - looks for files where the user permission has the SUID bit set (-u=s)

