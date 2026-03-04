# SMTP

- The following command, when executed, lists all the SMTP commands available in the
Nmap directory:  
`nmap -p 25, 365, 587 -script-smtp-commands <Target IP Address >`
- Run the following command to identify SMTP open relays:  
`nmap -p 25 -script-smtp-open-relay <Target IP Address>`
- Run the following command to enumerate all the mail users on the SMTP server:  
`nmap -p 25 -script-smtp-enum-users <Target IP Address>`

metasploit - `auxiliary/scanner/smtp/smtp_enum`

`python3 -m aiosmtpd -n -l <ip>:25` - we start a simple SMTP server to receive incoming emails.

	

username@[<ip>](@domen.thm - after `(` - comment all text and we send mail to our server (see more info https://portswigger.net/research/splitting-the-email-atom)
