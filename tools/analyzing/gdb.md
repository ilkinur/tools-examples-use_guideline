# GDB  

GNU Project Debugger and is a powerful debugging tool for C (along with other languages like C++). It helps you to poke around inside your C programs while they are executing and also allows you to see what exactly happens when your program crashes. GDB operates on executable files which are binary files produced by the compilation process.

| Command        | Description           |
| -------------  | ------------- |
| `run` or `r`      | Executes the program from start to end. |
| `break` or `b`      | Sets a breakpoint on a particular line.     |
| `disable` | Disables a breakpoint     |
| `enable` | Enables a disabled breakpoint. |
| `next` or `n` | Executes the next line of code without diving into functions. |
| `step` or `s` | Goes to the next instruction, diving into the function. |
| `list` or `l` | Displays the code. |
| `print` or `p` | Displays the value of a variable. | 
| `quit` or `q` | Exits out of GDB. |
| `clear` | Clears all breakpoints. | 
| `continue` or `c` | Continues normal execution |
| `info b` | View breakpoints | 
| `info functions` | Info abount functions | 
| `info functions func` | Info of the funtion |
| `info registers` | Value of the registers | 
| `disassemble` main | Disassemble the function |
| `set disassembly-flavor intel` | Show code in intel syntaxis |
| `set follow-fork-mode child/parent` |  Follow created process |
| `start` | Start and break in main |
| `set $eip = 0x12345678 ` | Change value of $eip |
| `bt` | Stack |
| `bt full` | Detailed stack | 
