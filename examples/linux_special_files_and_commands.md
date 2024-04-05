/etc/passwd - a colon-separated plaintext file that contains a list of the system's accounts and their attributes, such as the user ID (UID), group ID (GID), home directory location, and the login shell defined for the use

/etc/shadow - contains the password hashes of all users on the system.

/etc/group -  view all of the groups (and their respective group IDs) on the system



groups investigator - To determine which groups a specific user is a member of

getent group adm -  to list all of the members of a specific group

getent group 27  - To list all users in the sudo group, we can provide either the name "sudo" or the group ID, typically 27


last - display the history of the last logged-in users. It works by reading the /var/log/wtmp file, 

lastb - tracks failed login attempts by reading the contents of /var/log/btmp, which can help identify login and password attacks

The /var/log/auth.log file (or /var/log/secure on some distributions like CentOS or Red Hat) contains records of authentication-related events, including both successful and failed login attempts.


sudo cat /etc/sudoers - a particularly sensitive configuration file within Unix-like systems. It determines which users possess sudo privileges, enabling them to execute commands as other users, typically the root user.

debsums - If any files have been modified or corrupted, debsums will report them, citing potential issues with the package's integrity. This can be useful in detecting malicious modifications and integrity issues within the system's packages. sudo debsums -e -s we provide the -e flag to only perform a configuration file check. In addition, we provide the -s flag to silence any error output that may fill the screen.