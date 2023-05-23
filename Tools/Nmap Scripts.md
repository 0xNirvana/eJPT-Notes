#### Search for scripts
```
ls -la /usr/share/nmap/scripts | grep ftp-*
```
## SMB Nmap Scripts
- `smb-protocols`: Provides version information
- `smb-security-mode`: Provides information on guest account and message signing
- `smb-enum-sessions`: Determines SMB sessions
	- args (username, password): Optional
- `smb-enum-shares`: Provides details of available shares along with access-level information.
- `smb-server-stats`: Provides login stats details
- `smb-enum-domains`: Provides policy details like password min length
- `smb-enum-groups`:
- `smb-enum-users`: Provide list of users on the machine
- `smb-enum-services`:
- `smb-ls`: List everything in the SMB 
- `smb-os-discovery`: Determine the OS version
- `smb-vuln-ms17-010`: Check if vulnerable to EternalBlue
- Command
	```
	nmap -T4 -Pn -sC --script=smb-protocols,smb-security-mode,smb-enum-* --script-args smbusername=<username>,smbpass=<password> <target_ip>
	```

## FTP Nmap Scripts
- `ftp-brute`: brute-force FTP credentials
	- `--script-args userdb=<file_path>`
- `ftp-anon`:  Check if anonymous login is allowed

## SSH Nmap Scripts
- `ssh2-enum-algos`: list of compatible SSH alagorithms
- `ssh-hostkey`: get host keys
	- args: `ssh_hostkey=full`
- `ssh-auth-methods`: check authentication methods (public key or password)
	- args: `ssh.user=<username>`
- `ssh-brute`: brute-force passwords for ssh users
	- args: `userdb=<usernames_list>`
- `ssh-run`: run commands without logging in
	- args: `ssh-run.cmd=cat /home/user/flag,ssh-run.username=<username>,ssh-run.password=<password>`
## HTTP Nmap Scripts
- `http-enum`: version and some basic directories
- `http-headers`: returns basic header
- `http-methods`: get allowed methods
	- args: `http-methods.url-path=/<path>/`
- `http-webdav-scan`: provide the path as well as above
- `banner`: returns the banner

## MySQL Nmap Scripts
- `mysql-info`: get info about the db
- `mysql-variables`: get info about different variables that are set by MySQL
- `mysql-databases`: get list of databases
	- args: `mysqluser='<username',musqlpass='<pass>'`
- `mysql-audit`
	- args: `mysql-audit.user='<username>',mysql-audit.pass='<password>,mysql-audit.filename="/usr/share/nmap/nselib/data/mysql-cis.audit"`
- `mysql-dump-hashes`: dump hashes of user accounts on the machine
	- args: `username='<username>,password=<password>'`
- `mysql-query`: run a query
	- args: `query='<query>',username='<username>,password=<password>'`
- `mysql-empty-password`: check for anonymous login

## MSSQL Nmap Scripts
- `ms-sql-info`: get basic info on mssql
- `ms-sql-ntlm-info`: get ntlm details through mssql
- `ms-sql-brute`: bruteforce credentials
	- args: `userdb=<file_path>,passdb=<file_path>`
- `ms-sql-query`: send a query
	- args: `mssql.username=<uname>,mssql.password=<pass>,ms-sql-query="<query>"`
- `ms-sql-dump-hashes`: dump hashes of user accounts on the machine
	- args: `mssql.username='<username>,mssql.password=<password>'`
- `ms-sql-xp-cmdshell`: run command
	- args: `mssql.username='<username>,mssql.password=<password>',ms-sql-xp-cmdshell.cmd="<command>"`

## Vulnerabilities
### Linux
#### Shellshock
- `http-shellshock`
	- args: `http-shellshock.uri=/<.cgi file path>`

## Misc
### Banner Grabbing
- `banner`