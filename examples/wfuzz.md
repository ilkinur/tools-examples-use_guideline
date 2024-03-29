wfuzz -c -z file,/usr/share/wordlists/dirb/big.txt  --sc  200 http://$IP:5000/api/v2/resources/books?FUZZ=.bash_history

wfuzz -c -z file,/usr/share/wordlists/dirb/common.txt --hw 462,53  -H "Host: FUZZ.vulnnet.thm" http://vulnnet.thm/