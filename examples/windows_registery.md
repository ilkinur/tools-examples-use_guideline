# Windows Registery


###  Registry Hives

| Hive Name     | Contains                                                                 | Location                                                                 |
|--------------|--------------------------------------------------------------------------|--------------------------------------------------------------------------|
| **SYSTEM**   | - Services<br>- Mounted Devices<br>- Boot Configuration<br>- Drivers<br>- Hardware | `C:\Windows\System32\config\SYSTEM`                                      |
| **SECURITY** | - Local Security Policies<br>- Audit Policy Settings                     | `C:\Windows\System32\config\SECURITY`                                    |
| **SOFTWARE** | - Installed Programs<br>- OS Version and other info<br>- Autostarts<br>- Program Settings | `C:\Windows\System32\config\SOFTWARE`                                    |
| **SAM**      | - Usernames and their Metadata<br>- Password Hashes<br>- Group Memberships<br>- Account Statuses | `C:\Windows\System32\config\SAM`                                         |
| **NTUSER.DAT** | - Recent Files<br>- User Preferences<br>- User-specific Autostarts      | `C:\Users\username\NTUSER.DAT`                                           |
| **USRCLASS.DAT** | - Shellbags<br>- Jump Lists                                         | `C:\Users\username\AppData\Local\Microsoft\Windows\USRCLASS.DAT`         |

<br>
<br>

Example 1: View Connected USB Devices  
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USBSTOR`  
Example 2: View Programs Run by the User  
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU`  


The table below lists some registry keys that are particularly useful during forensic investigations.  

| Registry Key | Importance |
|-------------|------------|
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist` | It stores information on recently accessed applications launched via the GUI. |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths` | It stores all the paths and locations typed by the user inside the Explorer address bar. |
| `HKLM\Software\Microsoft\Windows\CurrentVersion\App Paths` | It stores the path of the applications. |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery` | It stores all the search terms typed by the user in the Explorer search bar. |
| `HKLM\Software\Microsoft\Windows\CurrentVersion\Run` | It stores information on the programs that are set to automatically start (startup programs) when the user logs in. |
| `HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs` | It stores information on the files that the user has recently accessed. |
| `HKLM\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName` | It stores the computer's name (hostname). |
| `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall` | It stores information on the installed programs. |
