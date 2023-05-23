## SMB
- Mount and unmount SMB on Windows
	```
	> net use Z: \\<IP_address>\C$ <password> /user:<username>
	> net use * /delete
	```
- Mount using GUI
	1. This PC -> Network -> Map Network Drive
	2. Enter the IP of target `\\<target IP>\` -> Browse -> Select the folder you want -> OK -> Finish

![[Nmap Scripts#SMB Nmap Scripts]]

### #tools/enum/smb/SMBMap
- Can be used with SMBv1 to get permission details and also run commands on the target machine
- Commands:
	```
	smbmap -u guest -p "" -d . -H <ip_addr> # get permmission details
	smbmap -u <username> -p <password -H <ip_address> -x <command> # execute a command remotely
	smbmap -u <username> -p <password -H <ip_address> -L # List all the attached drives
	smbmap -u <username> -p <password -H <ip_address> -r "<drive_label>$" # explore drive on the target machine
	smbmap -u <username> -p <password -H <ip_address>  --upload '<local file path>' '<remote_drive>$\path\filename' # Upload a file to remote server
	smbmap -u <username> -p <password -H <ip_address> --download '<drive_label>$\path'
	```

## Samba
- Runs on port 139 and 445
	- Can be discovered with #tools/multi/nmap 

### Tools
### #tools/multi/msfconsole 
- `auxiliary/scanner/smb/smb_version`
- `auxiliary/scanner/smb/smb2`
- `auxiliary/scanner/smb/smb_enumshares`
-  `auxiliary/scanner/smb/smb_login`
	- set `pass_file`
	- set `smbuser`
-  `auxiliary/scanner/smb/pipe_auditor`: Get named pipes
	- set username and passowrd
- Set rhost and run
### #tools/enum/smb/nmblookup 
- `-A <ip_address>`: List shares status on the target machine
### #tools/enum/smb/smbclient 
- `-L <ip_address>`: Provide shares detail
- `-N`: no password
- Check anonymous login
	```
	smbclient -L <ip> -N
	```
- Connect with a host
	```
	smbclient //<ip_addr> -N
	smbclient //<ip_addr>/<share_path> -U <username> # no $ at the end of share name
	```
### #tools/enum/smb/rpclient
- `-U "" -N <ip_address>`: Null session
	-  `srvinfo`: to get system details
	- `enumdomusers`: Get list of users
	- `enumdomgroups`: get list of groups
	- `lookupnames`: get SID of users
### #tools/enum/smb/enum4linux 
- `-o` : OS information
- `-U`: users
- `-S`: shares
- `-G`: groups
- `-r`: get SIDs
- `-i`: printer information

## FTP
- Storing files on server
- Default port 21
- Access directly using #tools/service/ftp
	```
	ftp <ip_address>
	```
- Password can be craked with #tools/bruteforce/hydra ![[Hydra#FTP]]
![[Nmap Scripts#FTP Nmap Scripts]]

## SSH
- Used for secure shell
- uses port 22
- Directly connect using #tools/service/ssh
	```
	ssh username@<ip_address>
	ssh -i <private_key_path> username@<ip_address>
	```
- ![[Nmap Scripts#SSH Nmap Scripts]]
### Cracking Password
- #tools/bruteforce/hydra ![[Hydra#SSH]]
- #tools/multi/metasploit![[Modules#SSH]]
## HTTP
- Port: 80
- Use #tools/recon/cli/whatWeb to find details about the server
- Use #tools/bruteforce/dirb to enumerate hidden paths on the server
- You can try to use #tools/enum/http/browsh to view the webpage in terminal
	```
	browsh --startup-url <ip_address>
	```
- Try using #tools/enum/http/curl to request the webpage on terminal
- Use #tools/enum/http/lynx which is similar to #tools/enum/http/browsh but more friendly display
- ![[Nmap Scripts#HTTP Nmap Scripts]]
- ![[Modules#HTTP]]
### Robots.txt
- Paths not allowed for robots

## MySQL
- Port: 3306
- Use #tools/service/mysql to directly connect to the server
	```
	mysql -h <ip_address> -u <username>
	```
- Some basic commands:
	- `show databases;`
	- `use`
	- `show tables;`
	- `select * from <table_name>`
- ![[Nmap Scripts#MySQL Nmap Scripts]]
- ![[Modules#MySQL]]
## MSSQL
- Port: 1433
- ![[Nmap Scripts#MSSQL Nmap Scripts]]
- ![[Modules#MSSQL]]
## SMTP
- Port: 25
- Connect with #tools/utility/nc to SMTP port
	```
	$ nc <ip_adr> 25
	# you will get the domain name and banner here from the target
	```
- Check if a user exists on the target machine
	```
	$ nc <ip_adr> 25
	VRFY <username>@domain_name
	```
- Check available options
	```
	# EHLO <target_domain_name>
	```
- Check for usernames existing on the target using #tools/enum/smtp/smtp-user-enum
	```
	smtp-user-enum -t <ip_addr> -p 25 -U <username_list>
	```
	- Same can be done with #tools/multi/metasploit 
- Send a fake email
	```
	root@attackdefense:~# nc 192.152.239.3 25
	220 openmailbox.xyz ESMTP Postfix: Welcome to our mail server.
	HELO openmailbox.xyz
	250 openmailbox.xyz
	mail from: attacker@attacker.xyz
	250 2.1.0 Ok
	rcpt to: admin@openmailbox.xyz
	250 2.1.5 Ok
	data
	354 End data with <CR><LF>.<CR><LF>
	Subject: Hi Root
	Fuck you bitch
	.
	250 2.0.0 Ok: queued as BFF3441DB94
	```
- Send fake email with #tools/exploit/smtp/sendemail
	```
	$ sendemail -f admin@attacker.xyz -t root@openmailbox.xyz -s 192.26.29.3 -u Fakemail -m "Hi root, a fake from admin" -o tls=no
	```

