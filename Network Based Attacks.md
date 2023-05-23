- Included following protocols
	- ARP
	- DHCP
	- SMB
	- FTP
	- Telnet
	- SSH
- Types of Attacks
	- MiTM

## Tshark
#tools/recon/network/tshark
- List all adapters
	```
	tshark -D
	```
- Open a PCAP file
	```
	tshark -r <filename>
	```
- Protocol Hierarchy
	```
	-z io,phs -q
	```
- Traffic between specific IPs
	```
	-Y 'ip.src=<source_ip> &&  ip.dst==<dst_ip>'
	```
- HTTP Traffic and display the time, source IP and full URI
	```
	-Y 'http.request.method==GET' -Tfields -e frame.time -e ip.src -e http.request.full_uri 
	```
- Regex in HTTP
	```
	-Y 'http contains password'
	```
- Request to specific destination
	```
	-Y 'http.request.method==GET && http.host==www.nytimes.com' -Tfields -e ip.dst
	```
- Retrieve cookies
	```
	-Y 'ip contains amazon.in && ip.src=<src_ip>' -Tfields -e ip.src -e http.cookie
	```
- Retrieve User-agemt
	```
	-Y 'ip.src==<src_ip> && http' -Tfields -e http.user_agent
	```

## ARP Poisoning
- Find your target hosts
- Enable IP Forwarding
	```
	echo 1 > /proc/sys/net/ipv4/ip_forward
	```
- Start #tools/exploit/network/arpspoof
	```
	arpspoof -i <interface> -t <target to spoof> -r <ip from where to capture packets>
	```

## WiFi Traffic Analysis
- Filter for open WiFi AP
	```
	(wlan.fc.type_subtype==0x0008) && !(wlan.wfa.ie.wpa.version==1) && !(wlan.tag.number==48)
	```
- Find channel or security mechanism
	```
	wlan contains <wifi name>
	```
- Find AP with MAC
	```
	wlan.addr == <mav>
	```

## Tshark for WiFi Analysis
- All wlan
	```
	-Y 'wlan'
	```
- Deauth
	```
	wlan.fc.type_subtype==0x000c'
	```
- EAPOL
	```
	-Y 'eapol'
	```
- SSID and MAC
	```
	-Y 'wlan.fc.type_subtype==8' -Tfields -e wlan.ssid -e wlan.bssid
	```
- BSSID of SSID
	```
	-Y 'wlan.ssid==<name>' -Tfields -e wlan.bssid
	```
