- Hashed passwords are stored in SAM (Security Account Manager) database
- Authentication and verification is performed by Local Security Authority (LSA)
- Upto Server 2003
	- LM (disabled Vista onwards)
	- NTLM
- SAM can't be copied while OS is running
- Windows NT kernel keeps SAM db file locked.
	- Hence in-memory attacks need to used to dump SAM hashes from LSASS process
- In modern Windows, SAM db is encrypted with a syskey
- Elevated ppriv is required to access and interact with LSASS

## LM (LanMan)
- Password broken into two seven-char chuunks
- All chars converted to uppercase
- Each chunk hashed separated with DES algo
- It is weak as it does not have a salt

## NTLM 
- Collection of authentication protocols
- When user is created, it is encrypted using the MD4 hashing and the original password is disposed

## Password in Config Files
- Unattended Windows Setup utlility used to automate the mass installation/deployment of Windows systems
	- Uses Administrator's account password
	- Can be found in:
		```
		C:\Windows\Panther\Unattend.xml
		C:\Windows\Panther\Autounattend.xml
		```

## Dumping Hashes w/ #tools/post/windows/Mimikatz
- Windows post exploitation tool
- Can be used to extract hashes from lsass.exe process memory
- There is a precompiled executable or we can use a meterpreter session and use Kiwi
- Need elevated privileges
### Manual Method
- Gain access to the machine with elevated privileges and upload the `mimikatz.exe` 
	```
	> upload /usr/share/windows-resources/mimikatz/x64/mimikatz.exe
	> .\mimikatz.exe
	> privilege::debug # should return OK
	> lsadump::sam # get hashes
	> lsadump::secret
	> sekurlsa::logonpasswords # clear text log on passwords
	```
### W/ Metasploit
![[Modules#Kiwi Module]]

## Pass the Hash Attack
- We can use #tools/exploit/multi/crackmapexec and #tools/exploit/psexec_py for this purpose
- Dump the hash as explained in [[Windows Credentials Dumping#Dumping Hashes w/ tools/post/windows/Mimikatz]]
- Take the TNLM hash that belongs to Administrator and use it with `psexec` ![[Modules#Psexec]]
- You can use #tools/exploit/multi/crackmapexec for the same as
	```
	# crackmapexec smb <target_ip> -u <username> -H <NTLM hash> -x "hash"
	```