# Upgrading Shells

`python -c 'import pty;pty.spawn("/bin/bash")'`  
`python3 -c 'import pty;pty.spawn("/bin/bash")'`  
`ctrl + z`  
`stty raw -echo`  
`fg`  
`Enter`  
`Enter`  
`export XTERM=xterm`  

Alternatively:  
`script -q /dev/null -c bash`  
`/usr/bin/script -qc /bin/bash /dev/null`  


