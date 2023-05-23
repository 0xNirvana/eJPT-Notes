## Mapping a Network
- Determine the scope
- Determine what you can acess and what not
- Determine if you'll get a VPN or physical access

#### Process
- Physical Access
	- Physical Security
	- OSINT
	- Social Engineering
- Sniffing
	- Passive Recon
	- Watch network traffic
- ARP
- ICMP
	- #tools/network/cli/traceroute
	- #tools/network/cli/ping 

#### Tools
- #tools/network/wireshark
- #tools/network/arp-scan
	- Determine devices in the network by sending ARP requests for each IP address.
	- `sudo arp-scan -I <interface> -g <IP range>`
- #tools/network/ping
	- `ping <ip address>`
- #tools/network/fping
	- Send multiple pings at the same time and see the hosts that respond to your ping
	- `fping -I <interface> -f <ip range>`
	- There might be hosts that won't respond to pings but will respond to ARP requests
- #tools/multi/nmap 
- #tools/network/zenmap
	- GUI version of `nmap`

### Port Scanning
- The best tool for this is #tools/multi/nmap but apart from that we have:
	- #tools/network/zenmap 
	- #tools/network/nmap-automator
	- #tools/network/masscan
	- #tools/network/rustscan
	- #tools/network/autorecon

- Some basic #tools/multi/nmap scan flags:
	- -sL: List Scan - simply list targets to scan
	- -sn: Ping Scan - disable port scan
	- -Pn: Treat all hosts as online -- skip host discovery
	- -sU: UDP Scan
	- -sN/sF/sX: TCP Null, FIN, and Xmas scans
		- When scanning systems compliant with this RFC text, any packet not containing SYN, RST, or ACK bits will result in a returned RST if the port is closed and no response at all if the port is open. As long as none of those three bits are included, any combination of the other three (FIN, PSH, and URG) are OK. Nmap exploits this with three scan types:
			- -sN: Does not set any bits
			- -sF: Sets just the TCP FIN bit.
			- -sX: Sets the FIN, PSH, and URG flags, lighting the packet up like a Christmas tree.
		- These three scan types are exactly the same in behavior except for the TCP flags set in probe packets. If a RST packet is received, the port is considered closed, while no response means it is open|filtered. The port is marked filtered if an ICMP unreachable error (type 3, code 0, 1, 2, 3, 9, 10, or 13) is received.
		- Can sneak through non-stateful f/w
	- -sS: TCP SYN Scan
		- Default scan
		- Does not complete the TCP handshake
		- Good when there are no f/w
		- Response:
			- SYN-ACK/SYN: Port open
			- RST: Closed
			- Nothing: Filtered
	- -sT: TCP Connect Scan
		- Default scan when SYN Scan is not available (when user does not have raw packet privilege)
		- Uses the `connect` system call to check ports
		- Can be caught by IDS and gets logged as well
		- The system call completes the connection as well
	- -sA: TCP ACK Scan
		- Not used to determine if ports is open
		- Used to determine the firewall policy and if they are stateful or not
	- -sW: Window scan is exactly the same as ACK scan except that it exploits an implementation detail of certain systems to differentiate open ports from closed ones, rather than always printing unfiltered when a RST is returned. It does this by examining the TCP Window field of the RST packets returned. On some systems, open ports use a positive window size (even for RST packets) while closed ones have a zero window. So instead of always listing a port as unfiltered when it receives a RST back, Window scan lists the port as open or closed if the TCP Window value in that reset is positive or zero, respectively.