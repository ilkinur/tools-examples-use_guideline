find / -user root -perm -4000 -exec ls -ldb {} \;


getcap -r / 2>/dev/null
