Search sub domains
`ffuf -w /usr/share/wordlists/dirb/big.txt -H "Host:FUZZ.nahamstore.thm" -u "http://nahamstore.thm" -fw 125`

`ffuf -w /usr/share/wordlists/dirb/big.txt -X POST -u http://host_ip/api/FUZZ -d "test=data" `
