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

#### -sO

Send IP package to target. If the target response with `RST` packages so the ports are open. If the target  don't response any package so the port will be `open-filtered`


### Anonim scanning

We need extra tools like `proxychains` and `tor`.
Start tor service  `service tor start`.  We can add extra proxy list to  `/etc/proxychains.conf` for speedly scan but there are default proxies if you don't have any proxy lists. Then scan anonymously like `proxychains nmap 192.168.181.0/24`  

`-D` <fakeip> - scan over fake IP.  
`-S` <spoofIP> - send packs like spoof ip and don't get any response because targent response to spoof ip  
`-mtu <number>` - to seerate packages for bypass firewall package limit


### Cheat Sheet (extra)

`nmap -iL hedefler.txt` - Dosyadaki hedeflerin toplu taraması.

`nmap -iR 100` - Belirli sayıda rastgele host taraması.

`nmap --exclude 192.168.1.25` - Dışlanmış IP ile tarama.

`nmap 192.168.1.0/24 -n` - DNS çözümlemesiz tarama.

`nmap 192.168.1.0/24 -sn` - Port taramasız ve isim çözümlemesiz.

`nmap 192.168.1.0/24 -PR` - ARP paketli ping taraması.

`nmap 192.168.1.0/24 -PN` - Pingsiz, sadece port taraması.

`nmap 192.168.1.0/24 -PA` - TCP ACK flag ile ping taraması.

`nmap 192.168.1.10 -F` - Hızlıca 100 portun taranması.

`nmap 192.168.1.10 --top-ports 1000` - En bilinen 1000 portun taranması. 

`nmap 192.168.1.10 -sV --versionintensity 8` - 0-9 arasındaki 8. hassasiyette versiyon taraması. Yüksek sayı = doğru sonuç

`nmap 192.168.1.10 -sV --version-light` - Hafif ve hızlı versiyon taraması.

`nmap 192.168.1.10 -sV --version-all` - En yüksek (9.) seviyede versiyon taraması.

`nmap 192.168.1.1 -O` - OS taraması. TCP/IP fingerprint ile.

`nmap 192.168.1.1 -O --osscan-limit` - TCP portu bulunan hedefler için tarama.

`nmap 192.168.1.1 -O --osscan-guess` - Daha agresif tahminler ile tarama.

`nmap 192.168.1.1 -O --max-os-tries 1` - Bir kez denemeli OS taraması.

`nmap 192.168.1.1 -A` - OS, versiyon ve güvenlik açığı taraması.

#### NSE (Nmap Script Engine) ilə Tarama

| Əmr | Açıqlama |
|-----|----------|
| `nmap 192.168.1.1 -sC` | Ən bilinən script’lərlə tarama. |
| `nmap 192.168.1.1 --script http-sql-injection` | Belirli bir script ilə tarama. |
| `nmap 192.168.1.1 --script smb*` | Belirli scriptlərin tümüylə tarama. |
| `nmap --script snmp-sysdescr --script-args snmpcommunity=admin 192.168.1.1` | Argümanlı script taraması. |
| `nmap 192.168.1.1 --script vuln` | Tüm scriptlər ilə tarama. |
| `nmap --script-updatedb` | Script veri bazasının güncellenməsi. <br> Scriptlər `/usr/share/nmap/scripts/` altındadır. |

#### Firewall və IDS (Intrusion Detection System) Atlatmalı Tarama

| Əmr | Açıqlama |
|-----|----------|
| `nmap 192.168.1.1 -f` | Parçalı IP paketləri ilə tarama. |
| `nmap 192.168.1.1 --mtu 16` | Paket boyutunu dəyişdirərək tarama. |
| `nmap -D RND:10` | Rastgele 10 ədəd decoy IP ilə tarama. |
| `nmap -D 192.168.1.5,192.168.1.6,192.168.1.7 192.168.1.1` | Belirli saxta (decoy) və çoxlu IP’lər ilə tarama. İçlərində biz də olmalıyıq. (`192.168.1.7`) |
| `nmap -S www.microsoft.com www.facebook.com` | Belirli saxta bir qaynaq ilə hədəfi tarama. |
| `nmap -g 53 192.168.1.1` | Belirli qaynaq port nömrəsindən tarama. |
| `nmap --proxies http://192.168.1.50:8080,http://192.168.1.20:80 192.168.1.1` | Belirli proxy’lər üzərindən tarama. |
| `nmap --data-length 25 192.168.1.1` | Göndərilən paketlərə rastgele olaraq 25 byte daha veri əlavə etməli tarama. |
