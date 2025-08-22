# Reconnaissance - Nmap

Nmap is a recon power tool for host and port scanning.

## Useful Options

- [Nmap options reference](https://nmap.org/book/man-briefoptions.html)
- `-Pn`: Disable ping scan. Forces nmap to scan all IPs/ports regardess of whether ICMP reponse is detected. Useful if hosts or firewalls are set to not response to ICMP requests.
- `-sS`: Syn scan. Default scan 
- `sU`: UDP scan - usefull 
- `T4`: Use a slightly faster/more aggressive scan than default T3 template
- `--packet-trace`: Use a packet trace. Useful for deep diving into 
- `-v`: Verbose mode. Adds some extra info and provides handy progress updates
-`--stats-every=5s`: Provide regular updates - useful for long running scans. Otherwise just press space while it's running.

## General purpose scans

- `nmap x.y.z.v/n -v -sn -oN 01_nmap_discovery.txt`: Host discovery. Scan a CIDR block to identify hosts. Disable port scanning (`-sn`) but add verbose mode to get a bit more output.
- `nmap $target_ip -sS -Pn -T4 -oN 01_nmap_synscan.txt`: Simple top 1000 ports using default SYN scan of a target host and ignoring ping results.
- `nmap $target_ip -p- --min-rate=1000 -Pn --diable-arp-ping -oN 01_nmap_full_synscan.txt`: Scan all ports - useful if you the above does not return expected results and you suspect more unusual ports may be present.
- `nmap $target_ip -sU -Pn -oN 01_nmap_udpscan.txt`: UDP scan. Try this is not much is present on tcp. Eg. (dns)

## Advanced scans
*Two stage scan*
This scan runs a broad range scan to identify open ports, then uses version detection and default scripts to further identify explore open ports.
```
ports=$(nmap $target_ip -p- --min-rate=1000 -T4 -oN 1_nmap-broad_scan| grep '^[0-9]' | cut -d '/' -f 1 | tr '\n' ',' | sed s/,$//)
nmap $target_ip -p $ports -sC -sV -oN 1_nmap_deep_scan.txt
```

*Firewall evasion*
- `nmap $target_ip -sA -Pn -T4 -oN 01_nmap_ackscan.txt`: Run an ACK scan. This only sends packets with an ACK flag, and is hard to filter out becuase the source/firewall cannot be sure the target did not initiate a connection.
- `nmap $target_ip -sS -Pn -T4 --source-port 53 -oN 01_nmap_dns_spoof.txt`: SYN-Scan from DNS Port. This might get better results if strict firewall rules are in place, but internal DNS traffic is still allowed.
