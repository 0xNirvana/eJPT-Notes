- It is the first phase in a pentest:
	- Passive
		- IP/DNS
		- Domain names & ownership information
		- Email addresses and social media profiles
		- Web technologies on target sites
		- Subdomains
	- Active
		- Open ports on target system
		- Learning the internal infrastructure
		- Enumerating information from target system

## Passive Recon
### Website Recon

- IP Address
	- Website can be hosted behind a proxy or cloud provider like CloudFare
	- #tools/recon/cli/host:
	    ```
	    ntayade@macos-K7CXYG74CW Internal Recon % host hackersploit.org
	    hackersploit.org has address 172.67.202.99
	    hackersploit.org has address 104.21.44.180
	    hackersploit.org has IPv6 address 2606:4700:3036::ac43:ca63
	    hackersploit.org has IPv6 address 2606:4700:3031::6815:2cb4
	    hackersploit.org mail is handled by 0 _dc-mx.2c2a3526b376.hackersploit.org.
	    ```
		- The twoIPv4/IPv6 addresses suggest that this website is behind a firewall/proxy.
- Hidden directories
	- `/robots.txt`
		- This disallows crawler to index certain files, so that they don't show up with search engines
	- `sitemap.xml`
		- Provides search engines an organized way to access the website - contains all the accessible links on the website.
- Names
- Emails
- Phone numbers
- Physical Addresses
- Web technologies being used
	- #tools/recon/browser/builtWith 
		- Also tell you about some of the subdomains
	- #tools/recon/browser/wappalyzer
	- #tools/recon/cli/whatWeb 
		- Redirections
		- Proxy/Firewall
		- IP
		- Title
		- Uncommon headers
		- Languages and versions
	- #tools/recon/cli/HTTrack 
		- To download webpages entirely

### Whois Enumeration
- #tools/recon/cli/whois 
- It is an internet protocol used for querying databases that store registered users or asignees of an internet resource.
- Execution
	```
	ntayade@macos-K7CXYG74CW .ssh % whois zonetransfer.me
	% IANA WHOIS server
	% for more information on IANA, visit http://www.iana.org
	% This query returned 1 object
	
	refer:        whois.nic.me
	
	domain:       ME
	
	organisation: Government of Montenegro
	address:      Rimski trg 46
	address:      Podgorica 81000
	address:      Montenegro
	
	contact:      administrative
	name:         Božo Krstajić
	organisation: University of Montenegro, Faculty of Electrical Engineering
	address:      Džordža Vašingtona bb
	address:      Podgorica 81000
	address:      Montenegro
	phone:        +382 67 313 222
	e-mail:       bozok@ac.me
	
	contact:      technical
	name:         TLD Tech
	organisation: Center of Information System (CIS)
	organisation: University of Montenegro
	address:      Cetinjski put 2
	address:      Podgorica 81000
	address:      Montenegro
	phone:        +382 20 414 290
	fax-no:       +382 20 414 283
	e-mail:       tldtech@ac.me
	
	nserver:      A0.NIC.ME 199.253.59.1 2001:500:53:0:0:0:0:1
	nserver:      A2.NIC.ME 199.249.119.1 2001:500:47:0:0:0:0:1
	nserver:      B0.NIC.ME 199.253.60.1 2001:500:54:0:0:0:0:1
	nserver:      B2.NIC.ME 199.249.127.1 2001:500:4f:0:0:0:0:1
	nserver:      C0.NIC.ME 199.253.61.1 2001:500:55:0:0:0:0:1
	ds-rdata:     45352 8 2 7708c8a6d5d72b63214bbff50cb54553f7e07a1fa5e9074bd8d63c43102d8559
	
	whois:        whois.nic.me
	
	status:       ACTIVE
	remarks:      Registration information: http://www.domain.me
	
	created:      2007-09-24
	changed:      2020-10-19
	source:       IANA
	
	# whois.nic.me
	
	Domain Name: ZONETRANSFER.ME
	Registry Domain ID: D108500000003513097-AGRS
	Registrar WHOIS Server:
	Registrar URL: http://www.meshdigital.com
	Updated Date: 2022-01-05T10:14:50Z
	Creation Date: 2011-12-27T15:34:08Z
	Registry Expiry Date: 2023-12-27T15:34:08Z
	Registrar Registration Expiration Date:
	Registrar: Mesh Digital Limited
	Registrar IANA ID: 1390
	Registrar Abuse Contact Email:
	Registrar Abuse Contact Phone:
	Reseller:
	Domain Status: ok https://icann.org/epp#ok
	Registrant Organization: DigiNinja
	Registrant State/Province: Routerville
	Registrant Country: GB
	Name Server: NSZTM1.DIGI.NINJA
	Name Server: NSZTM2.DIGI.NINJA
	DNSSEC: unsigned
	URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
	>>> Last update of WHOIS database: 2023-03-31T01:41:48Z <<<
	```
	- Something important over here is the Name Server. It could be owned by the owner itself or could be a cloud provider like CloudFlare.
- You can also use websites to get the same information.
- If you find contact details here then you can use them for phishing attacks as well
- You can also perform a `whois` lookup against an IP address and it will give you details like the IP block owned by the provider and many other details.
	```
	ntayade@macos-K7CXYG74CW .ssh % whois 104.21.44.180
	% IANA WHOIS server
	% for more information on IANA, visit http://www.iana.org
	% This query returned 1 object
	
	refer:        whois.arin.net
	
	inetnum:      104.0.0.0 - 104.255.255.255
	organisation: ARIN
	status:       ALLOCATED
	
	whois:        whois.arin.net
	
	changed:      2011-02
	source:       IANA
	
	# whois.arin.net
	
	NetRange:       104.16.0.0 - 104.31.255.255
	CIDR:           104.16.0.0/12
	NetName:        CLOUDFLARENET
	NetHandle:      NET-104-16-0-0-1
	Parent:         NET104 (NET-104-0-0-0-0)
	NetType:        Direct Allocation
	OriginAS:       AS13335
	Organization:   Cloudflare, Inc. (CLOUD14)
	RegDate:        2014-03-28
	Updated:        2021-05-26
	Comment:        All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
	Ref:            https://rdap.arin.net/registry/ip/104.16.0.0
	
	
	
	OrgName:        Cloudflare, Inc.
	OrgId:          CLOUD14
	Address:        101 Townsend Street
	City:           San Francisco
	StateProv:      CA
	PostalCode:     94107
	Country:        US
	RegDate:        2010-07-09
	Updated:        2021-07-01
	Ref:            https://rdap.arin.net/registry/entity/CLOUD14
	
	
	OrgNOCHandle: CLOUD146-ARIN
	OrgNOCName:   Cloudflare-NOC
	OrgNOCPhone:  +1-650-319-8930 
	OrgNOCEmail:  noc@cloudflare.com
	OrgNOCRef:    https://rdap.arin.net/registry/entity/CLOUD146-ARIN
	
	OrgAbuseHandle: ABUSE2916-ARIN
	OrgAbuseName:   Abuse
	OrgAbusePhone:  +1-650-319-8930 
	OrgAbuseEmail:  abuse@cloudflare.com
	OrgAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE2916-ARIN
	
	OrgTechHandle: ADMIN2521-ARIN
	OrgTechName:   Admin
	OrgTechPhone:  +1-650-319-8930 
	OrgTechEmail:  rir@cloudflare.com
	OrgTechRef:    https://rdap.arin.net/registry/entity/ADMIN2521-ARIN
	
	OrgRoutingHandle: CLOUD146-ARIN
	OrgRoutingName:   Cloudflare-NOC
	OrgRoutingPhone:  +1-650-319-8930 
	OrgRoutingEmail:  noc@cloudflare.com
	OrgRoutingRef:    https://rdap.arin.net/registry/entity/CLOUD146-ARIN
	
	RTechHandle: ADMIN2521-ARIN
	RTechName:   Admin
	RTechPhone:  +1-650-319-8930 
	RTechEmail:  rir@cloudflare.com
	RTechRef:    https://rdap.arin.net/registry/entity/ADMIN2521-ARIN
	
	RNOCHandle: NOC11962-ARIN
	RNOCName:   NOC
	RNOCPhone:  +1-650-319-8930 
	RNOCEmail:  noc@cloudflare.com
	RNOCRef:    https://rdap.arin.net/registry/entity/NOC11962-ARIN
	
	RAbuseHandle: ABUSE2916-ARIN
	RAbuseName:   Abuse
	RAbusePhone:  +1-650-319-8930 
	RAbuseEmail:  abuse@cloudflare.com
	RAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE2916-ARIN
	```

### Website Footprinting with #tools/recon/browser/netcraft
- Website: https://netcraft.com
-  Provides the following info
	- Hosting information
	- Network information
	- IP details - block-wise
	- SSL/TLS certificate details
	- Vulnerabilities related to SSLv3/POODLE
	- Hosting history (could be redacted if using service like CloudFlare)
	- Analytics
	- Tech stack

### DNS Recon
#### #tools/recon/cli/dnsrecon
- Use is bascially to get details like NS records, A/AAAA records and MX records
- Tells you if DNSSEC is configured or not
#### #tools/recon/browser/dnsdumpster 
- IP block owners
- HTTP headers
- Zone Transfers
- Other hosts that are using this DNS server
- MX records

### WAF Detection #tools/recon/cli/wafw00f
- Analyzes HTTP response headers and determines the WAF
- Can be helpful when you application is behind a WAF, this can help you to determine your next steps based on the WAF in-line.
- Based on the result from wafw00f and combining the same with results from DNS recon, you can determine if the IP address is the actual target's address or it is being protected by a proxy.

### Subdomain Enumeration with #tools/recon/cli/sublist3r
- You can also brute-force subdomains with sublist3r
- This tools makes a lot of requests, so you might get blocked by services like google (but you can bypass this by VPN)

### Google Dorks
- #tools/recon/browser/googledorks
- `site:` Restrict to a specific domain
- `inurl:` Make sure a word is in the URL
- Subdomains:
	- `site:*.ine.com`
- `intitle:`
- `filetype:`
- Directory Listing (this should not be enabled on websites)
	- `intitle:index of`
- `inurl:passwd.txt`
- Exploit Database

### #tools/recon/cli/theHarverster
- Get publicly exposed information like:
	- emails
	- contact details
	- host names and IP addresses

### Leaked Password Databases
- haveibeenpwned.com
	- Check password here and check if they can been reused with your target's other accounts

## Active Recon
### DNS Zone Transfer
- You can find you default DNS server on your router's configuration
- You can also set a custom DNS server like 1.1.1.1 or 8.8.8.8
- If DNS zone transfer is left unsecured, it can be abused to copy zone file from Primary DNS to another DNS server which can provide the attacker a holistic view of an organization's network layout.
- #tools/recon/cli/dnsenum can  be used for passive DNS recon as well as zone transfer
> You can add an entry like 192.168.1.1 router.admin in your `/etc/hosts` file then visit that particular IP with the domain name entered in the file
- You can also use #tools/recon/cli/dig with `axfr` switch to perform zone transfer.
- DNSrecon might start bruteforcing subdomains automatically, hence `dig` is better as it just tries to do a zone transfer
- #tools/recon/cli/fierce is another tools that helps locate non-contiguous IP space and hostnames against domains

### #tools/multi/nmap 
- The first set is to discover hosts on a network. This can be done with a ping sweep (`-sn` i.e. no port scan):
	```
	$ sudo nmap -sn <IP range>
	```
- The same discovery can be made with #tools/recon/cli/netdisover but instead of sending pings, this sends ARP requests to find hosts in the network
	```
	$ netdiscover -i <interface_name> -r <IP range>
	```
- `nmap` runs a default SYN scan. 
- You can add `-Pn` if you think the machine is blocking ping probes
- A `Filtered State` could mean there is a firewall inplace blocking/filtering the requests.
- `nmap` performs TCP scan by default, but a UDP scan can be done by with `-sU`
- `-sV`: service version detection
- `-F`: fast scan
- `-O`: OS Scan
- `-sC`: script scans
- `-A`: aggresive (combines `-sV`, `-sC` and `-A`)