# NMAP
### Nmap Scan Techniques

SWITCH |	EXAMPLE |	DESCRIPTION 
|----------------|--------------|----------|
`-sS` |	nmap 192.168.1.1 -sS |	TCP SYN port scan (Default)
`-sT` |	nmap 192.168.1.1 -sT |	TCP connect port scan (Default without root privilege)
`-sU` |	nmap 192.168.1.1 -sU | UDP port scan
`-sA` |	nmap 192.168.1.1 -sA |	TCP ACK port scan
`-sW` |	nmap 192.168.1.1 -sW |	TCP Window port scan
`-sM` |	nmap 192.168.1.1 -sM |	TCP Maimon port scan
`-sF` |	nmap 192.168.1.1 -sF |	Fin scan
`-sP` |	nmap 192.168.1.1 -sP |	Ping scan
`-sV` |	nmap 192.168.1.1 -sV |	Version dedection
`-sO` |	nmap 192.168.1.1 -sO |	IP protocol scan

#### -sS

Send `TCP SYN` package to target, if port of the target is open response with `SYN-ACK` and attacker send `RST` package.

#### -sA

Send `TCP ACK` package to target, if the target doesn't any response or response with `ICMP Destination Unreachable` port will be `filtered`. If the target response with RST package the port will be `unfiltered`.

#### -sT

Send `TCP SYN` package to target, if port of the target is open response with `SYN-ACK` (if the port is close response will be `RST-ACK`) and attacker send `ACK` package (three way handshake is completed).

#### -sP

Send one package to target and target will be only response `ICMP Echo` (if don't have any filter)

#### -sU

Send UDP package to target. If the target response with `UDP` packages so the ports are open. If the target response `ICMP Port Unreachable` or don't response any package so the port will be `open-filtered`
