## Import Nmap Scan Results in MSF
- Start #tools/multi/nmap scan with `-oX` and store data in an XML file
- Start #tools/multi/metasploit 
- Run the command
	```
	> db_import <xml_filename>
	> hosts # ensure that file has been loaded
	```
- You can run nmap scans from within MSF
	```
	> db_nmap <nmap command>
	``` 
	- This will automatically store the result in MSF as well

## Port Scanning w/ Auxiliary Modules
- Auxiliary modules can be use to discover open port on a different interface on the target machine that we have initial access on. All you need to do is create a route first from the session like:
	```
	meterpreter> run autoroute -s <ip address of the interface>
	```
- #tools/multi/nmap can't be used in this way as you'll need to send the binary on the target machine first.
- Useful modules (set rhosts as the IP address of other interface):
	- `auxiliary/scanner/portscan/tcp` 
	- `auxiliary/scanner/discovery/udp_sweep`

## Useful Modules
![[Modules#Services]]


