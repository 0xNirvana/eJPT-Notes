*This checklist is based on the eJPT PTS training material. It can be updated as per requirement.*
## Assessment Methodologies
### [[Information Gathering]]
#### [[Information Gathering#Passive Recon |Passive Recon]]
- [ ] #tools/recon/cli/host 
- [ ] `./robots.txt`
- [ ] `./sitemap.xml`
- [ ] #tools/recon/browser/builtWith 
- [ ] #tools/recon/browser/wappalyzer 
- [ ] #tools/recon/cli/whatWeb 
- [ ] #tools/recon/cli/whois 
- [ ] #tools/recon/browser/netcraft 
- [ ] #tools/recon/cli/dnsrecon 
- [ ] #tools/recon/browser/dnsdumpster 
- [ ] #tools/recon/cli/wafw00f 
- [ ] #tools/recon/cli/sublist3r 
- [ ] #tools/recon/browser/googledorks 
- [ ] #tools/recon/cli/theHarverster 

#### [[Information Gathering#Active Recon|Active Recon]]
- [ ] #tools/recon/cli/dnsenum 
- [ ] #tools/recon/cli/dig 
- [ ] #tools/recon/cli/fierce 
- [ ] #tools/recon/cli/netdisover 

### [[Footprinting & Scanning]]
- [ ] #tools/network/cli/traceroute 
- [ ] #tools/network/fping 
- [ ] #tools/multi/nmap [[Nmap Scripts]]

### [[Enumeration]]
- [ ] #tools/bruteforce/hydra 

#### [[Enumeration#SMB|SMB]]
- [ ] #tools/enum/smb/enum4linux 
- [ ] #tools/enum/smb/SMBMap 
- [ ] #tools/enum/smb/nmblookup 
- [ ] #tools/enum/smb/smbclient 
- [ ] [[Nmap Scripts#SMB Nmap Scripts|Nmap Scripts]]
- [ ] [[Modules#SMB|MSFconsle Modules]]

#### [[Enumeration#FTP|FTP]]
- [ ] #tools/service/ftp 
- [ ] [[Nmap Scripts#FTP Nmap Scripts|Nmap Scripts]]

#### [[Enumeration#SSH|SSH]]
- [ ] #tools/service/ssh 
- [ ] [[Nmap Scripts#SSH Nmap Scripts|Nmap Scripts]]
- [ ] [[Modules#SSH|MSFconsole Modules]]

#### [[Enumeration#HTTP|HTTP]]
- [ ] [[Nmap Scripts#HTTP Nmap Scripts|Nmap Scripts]]
- [ ] [[Modules#HTTP|MSFconsole Modules]]

#### [[Enumeration#MySQL|MySQL]]
- [ ] #tools/service/mysql 
- [ ] [[Nmap Scripts#MySQL Nmap Scripts|Nmap Scripts]]
- [ ] [[Modules#MySQL|MSFconsole Modules]]

#### [[Enumeration#MSSQL|MSSQL]]
- [ ] [[Nmap Scripts#MSSQL Nmap Scripts]]
- [ ] [[Modules#MSSQL|MSFconsole Modules]]

#### [[Enumeration#SMTP|SMTP]]
- [ ] #tools/enum/smtp/smtp-user-enum 
- [ ] #tools/exploit/smtp/sendemail 
- [ ] [[Modules#SMTP|MSFconsole Modules]]

### [[Vulnerability Assessment]]
- [ ] Nessus
- [ ] ExploitDB
- [ ] #tools/multi/msfconsole 

## [[Vulnerability Assessment#Auditing|Auditing]]
- [ ] SCAP Compliance Checker
- [ ] #tools/multi/nmap 
- [ ] Nessus

## Host & Network Penetration Testing
### System/Host Based Attacks
- [ ] Determine hosts with #tools/network/fping 
- [ ] #tools/multi/nmap 
- [ ] [[Nmap Scripts]]
- [ ] #tools/multi/msfconsole 
- [ ] [[Modules|MSFconsole Modules]]
### Exploitation
- [ ] #tools/exploit/searchsploit 
- [ ] #tools/utility/nc 
- [ ] #tools/payloads/msfvenom 
### Post-Exploitation
#### Windows
- [ ] [[Post Exploitation/Windows/Post Exploitation|Manual Enumeration]]
- [ ] #tools/multi/msfconsole 
- [ ] [[Post Exploitation/Windows/PrivEsc|PrivEsc Check]] 
	- [ ] [[Windows Post Exploitation#Common post exploitation modules|MSFconsole Post Modules]]
	- [ ] [[Windows Post Exploitation#UAC Bypass|UAC Bypass]]
	- [ ] [[Windows Post Exploitation#Token Impersonation|Token Impersonation]]
	- [ ] [[Windows Post Exploitation#Dumping Hashes w/ Kiwi Extension|Dumping Hashes]]
- [ ] #tools/exploit/psexec_py 
- [ ] #tools/enum/windows/Windows-Exploit-Suggester 
- [ ] #tools/exploit/windows/UACMe 
- [ ] #tools/post/windows/Mimikatz 
- [ ] [[Post Exploitation/Windows/Persistence|Persistence via Service & RDP]]
- [ ] 
- [ ] [[Post Exploitation/Linux/Clearing Tracks|Clearing Tracks]]

#### Linux
- [ ] [[Post Exploitation/Linux/Post Exploitation|Manual Enumeration]]
- [ ] #tools/enum/smb/enum4linux 
- [ ] #tools/privesc/linux/linenum 
- [ ] #tools/enum/linux/linux-exploit-suggester 
- [ ] [[Linux Privilege Escalation#Misconfigured Cron Jobs|Cron Jobs]]
- [ ] [[Linux Privilege Escalation#SUID Binaries|SUID Binaries]]
- [ ] [[Linux Privilege Escalation#Dumping Credentials|Dumping Credentials]]
- [ ] [[Post Exploitation/Linux/Persistence|Persistence via SSH & CRON]]
- [ ] [[Post Exploitation/Linux/Clearing Tracks]]
- [ ] #tools/multi/msfconsole 
	- [ ] [[Linux Post Exploitation#Modules|Linux Post Modules]]
	- [ ] [[Linux Post Exploitation#Dumping Hashes w/ Hashdump|Dumping Hashes]]
	- [ ] [[Linux Post Exploitation#Persistence on Linux|Persistence]]

## Web Application Penetration Testing
- [ ] #tools/utility/curl 
- [ ] #tools/web/gobuster 
- [ ] #tools/web/burpsuite 
- [ ] #tools/web/ZAProxy 
- [ ] #tools/web/nikto 
- [ ] #tools/web/sqlmap 
- [ ] #tools/web/xsser 

## Others
### Network Based Attacks
- [ ] #tools/recon/network/tshark 
- [ ] #tools/exploit/network/arpspoof 

### Social Engineering
- [ ] #tools/social/gophish 

## Debug Tools
- [ ] #tools/network/ping 
- [ ] #tools/network/wireshark 
- [ ] #tools/network/nmap-automator 
- [ ] #tools/network/masscan 
- [ ] #tools/network/rustscan 
- [ ] #tools/network/autorecon 