## Linux
- Recurcsive grep
	```
	grep -rnw /<path_where to look> -e <string to be looked for> 2> /dev/null
	```
- Get version and OS details
	```
	cat /etc/*release
	vat /etc/*issue
	uname -a
	uname -r
	```
- Get a `bash` shell:
	```
	/bin/bash -i
	```
- Start a webserver
	- Python2
		```
		python -m SimpleHTTPServer 80
		```
	- Python3
		```
		python3 -m http.server 80
		```
- Download files
	```
	wget <url>
	```

## Windows
- Download file (to C:\\Temp)
	- Certutil:
		```
		certutil -urlcache -f http://<remote_address> <local_name>
		```
	- PowerShell DownloadFile:
	```
	`(new-object System.Net.WebClient).DownloadFile('http://www.xyz.net/file.txt','C:\tmp\file.txt')`
	```
- Check users
	```
	net users // users in 
	net localgroup Administrator // members in Admininstrator group
	```
- `?`: Use this to check what options are there with the command
- 