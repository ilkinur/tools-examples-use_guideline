# Linux Incident Response

### /etc/passwd
To identify whether there is an account entry in your system that may seem suspicious. This command usually fetches all the information about the user account.  
`cat /etc/passwd`

### passwd -S
The ‘Setuid’ option in Linux is unique file permission. So, on a Linux system when a user wants to make the change of password, they can run the ‘passwd’ command. As the root account is marked as setuid, you can get temporary permission  
`passwd -S raj`

### grep
Grep is used for searching plain- text for lines that match a regular expression. :0: is used to display ‘UID 0’ files in /etc/passwd file.  
`grep :0: /etc/passwd`

### find /-nouser
To Identify and display whether an attacker created any temporary user to perform an attack, type  
`find / -nouser -print` 

### /etc/shadow
The /etc/shadow contains the encrypted password, details about the passwords and is only accessible by the root users  
`cat /etc/shadow`

### /etc/group
The group file displays the information of the groups used by the user. To view the details, type  
`cat /etc/group`

### /etc/sudoers
If you want to view information about user and group privileges to be displayed, the/ etc/sudoers file can be viewed   
`cat /etc/sudoers`

### Lastlog
To view the reports of the most recent login of a particular user or all the users in the Linux system, you can type  
`lastlog`

### Auth.log
To identify any curious SSH & telnet logins or authentication in the system, you can go to /var/log/ directory and then type  
`tail auth.log` (also you can see auth.log with `ausearch`)

### History
To view the history of commands that the user has typed, you can type history with less or can even mention up to the number of commands you typed last. To view history, you can type  
`history| less`

### Uptime
To know whether your Linux system has been running overtime or to see how long the server has been running for, the current time in the system, how many users have currently logged on, and the load averages of the system, then you can type:  
`uptime`

### Free
To view the memory utilisation by the system in Linux, the used physical and swap memory in the system, as well as the buffers used by the kernel, you can type  
`free`

### /proc/memory
As an incident responder to check the detail information of the ram, memory space available, buffers and swap on the system, you can type  
`cat /proc/meminfo`

### /proc/mounts
As an incident responder, it’s your responsibility to check if there is an unknown mount on your system, to check the mount present on your system, you can type  
`cat /proc/mounts`

### top
To get a dynamic and a real-time visual of all the processes running in the Linux system, a summary of the information of the system and the list of processes and their ID numbers or threads managed by Linux Kernel, you can make use of   
`top`

### ps aux
To see the process status of your Linux and the currently running processes system and the PID. To identify abnormal processes that could indicate any malicious activity in the Linux system, you can use  
`ps aux`

### PID
To display more details on a particular process, you can use  
`lsof –p [pid]`

### Service
To find any abnormally running services, you can use  
`service –-status-all`

### /etc/cronjob
The incident responder should look for any suspicious scheduled tasks and jobs. To find the scheduled tasks, you can use  
`cat /etc/crontab`

### /etc/resolv.conf
To resolve DNS configuration issues and to avail a list of keywords with values that provide the various types of resolver information, you can use  
`more /etc/resolv.conf`

### /etc/hosts
To check file that translates hostnames or domain names to IP addresses, which is useful for testing changes to the website or the SSL setup, you can use  
`more /etc/hosts`

### iptables
To check and manage the IPv4 packet filtering and NAT in Linux systems, you can use iptables and can make use of a variety of commands like:  
`iptables -L -n`

### Large Files
To identify any overly large files in your system and their permissions with their destination, you can use  
`find /home/ -type f -size +512k -exec ls -lh {} \;`

### mtime
As an incident responder, if you want to see an anomalous file that has been present in the system for 2 days, you can use the command  
`find / -mtime -2 -ls`

### ifconfig
To obtain the network activity information, you can use various commands.  
`ifconfig`  
To see all the network interfaces, you can use  
`ifconfig -a`

### Open files
To list all the processes that are listening to ports with their PID, you can use  
`lsof -i`

### netstat
To display all the listening ports in the network use  
`netstat -nap`

### arp
To display the system ARP cache, you can type  
`arp -a`

### path
The $PATH displays a list of directories that tells the shell which directories to search for executable files, to check for directories that are in your path you can use  
`echo $PATH`

------------------------------------------------

# Windows Incident Response

### Local users
To view the local user accounts in GUI, press `Windows+R`, then type `lusrmgr.msc`

### net user
You can now open the command prompt and run it as an administrator. Then type the command `net user` and press enter. You can now see the user accounts for the system and the type of account it is.  
`net user`

### net localgroup
‘Net localgroup groupname’ command is used to manage local user groups on a system. By using this command, an administrator can add local or domain users to a group, delete users from a group, create new groups and delete existing groups.  
`net local group administrators`

### Local user
To view the local user accounts in PowerShell, open PowerShell as an administrator, type ‘GetLocalUser’ and press enter. You will be able to see the local user accounts, with their names, if they are enabled and their description  
`Get-LocalUser`

### Task Manager
To view the running processes in a GUI, press `Windows+R`, then type `taskmgr.exe`

### tasklist
To view the process list in PowerShell, run PowerShell as an administrator and type ‘Get-Process’ and press enter. It gets a list of all active processes running on the local computer.  
`get-process`

Windows system has an extremely powerful tool with the Windows Management Instrumentation Command (WMIC)  
`wmic process list full`

To get more details about the parent process IDs, Name of the process and the process ID, open PowerShell as an administrator and type ‘wmic process get name,parentprocessid,processid’. This would be the next step after you determine which process is performing a strange network activity. You will see the following details.  
`wmic process get name,parentprocessid,processid`

To get the path of the Wmic process, open PowerShell and type ‘wmic process where 'ProcessID=PID’ get Commandline’ and press enter.  
`wmic process where 'ProcessID=PID’ get Commandline`

### Services
To view all the services in GUI, press `Windows+R` and type `services.msc`.  

### net start
To start and view the list of services that are currently running in your system, open the command prompt as an administrator, type ‘net start’ and press enter  
`net start`

### sc query
To view whether a service is running and to get its more details like its service name, display name, etc.  
`sc query | more`

### tasklist
If you want a list of running processes with their associated services in the command prompt, run command prompt as an administrator, then type ‘tasklist /svc’ and press enter.  
`tasklist /svc`

### Schtasks
To view the schedule tasks in the command prompt, run command prompt as an administrator, type ‘schtasks’ and press enter.  
`schtasks`

### Startup
`dir /s /b "C:\Users\username\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup"`

To view, the startup applications in the PowerShell run the PowerShell as an administrator  
`wmic startup get caption,command`

To get a detailed list of the AutoStart applications in PowerShell , you can run it as an administrator  
`Get-CimInstance Win32_StartupCommand | Select-Object Name, command, Location, User | Format-List’`

### Registry
You can also view the registry of the Local Machine of the Run key in the PowerShell, by running it as an administrator and then type  
`reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`  
`reg query HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

### netstat
`netstat –ano`  
`Get-NetTCPConnection -LocalAddress 192.168.0.110 | Sort-Object LocalPort`

### net view
`net view \\127.0.0.1`  

### SMBShare
`Get-SMBShare`  

### Forfiles
To view the .exe files with their path to locate them in the command prompt  
`forfiles /D -10 /S /M *.exe /C "cmd /c echo @path"`  

To View files without its path and more details of the particular file extension and its modification date  
`forfiles /D -10 /S /M *.exe /C "cmd /c echo @ext @fname @fdate"`  

To check for files modified in the last 10 days  
`forfiles /p c: /S /D -10`  

### Firewall Settings
To view the firewall configurations in the command prompt  
`netsh firewall show config`  

To view the firewall settings of the current profile in the command prompt  
`netsh advfirewall show currentprofile`  

### Sessions with other system
To check the session details that are created with other systems  
`net use`  

### Open Sessions
You can type ‘net session’ in the command prompt and press enter to see any open sessions of your system. It gives you the details about the duration of the session.  
`net session`

### Event Viewer
To export certain logs of a particular event in command prompt type ‘wevtutil qe security’ and press enter  
`wevtutil qe security`

To get the event log list in the PowerShell, type ‘Get-EventLog -list’ and type the particular event in the supply value and you will get event details of that particular event.  
`Get-Eventlog -List`








