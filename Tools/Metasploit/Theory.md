### Front-ends
- Armitage: Java-based GUI frontend
- Metasploit Community Edition: Web based GUI front-end
- MSFconsole
- MSFcli

### Architecture
#### Modules
- Piece of code that Metasploit executes
- **Exploit:** Used to take advantage of a vulnerability and is paired with a payload
- **Payload:** Code that is delivered by MSF
- **Encoder:** Used to encode payload to avoid AV detectikon
- **NOPS:** Used to ensure that payload sizes are consistent
- **Auxiliary:** Module for additional functionality other than exploitation

#### Payload Types
Depend on the target system and type
- **Non-Staged Payload:** Sent to the target along with the exploit
- **Staged Payload:** Sent to the target in two parts
	- **Stager: **First part creates a reverse connection with the attacker
	- **Stage:** Second part contains the exploit payload and executes it

#### Meterpreter Payload
- Meta-Interpreter is an advanced multi-functional payload that is executed in memory on the target making it difficult to get detected 

#### MSF File system
- `/usr/share/metasploit-framework`
- Custom modules: `~/.ms4/modules`

#### Using MSF w/ Penetration Tests
| Phase | Module |
|-----|-----|
| Information Gathering | Auxiliary |
| Vulnerability Scanning | Auxiliary |
| Exploitation | Exploit Modules & Payloads |
| Post Exploitations | Meterpreter |
| Privilege Escalation | Post Exploitation Meterpreter |
| Persistence | Post Exploitation Meterpreter |

### MSFconsole Fundamentals
- **Common Variables**
	- LHOST
	- LPORT
	- RHOST/S
	- RPORT
- **Common Commands**
	- `help`
	- `version`
	- `show <all|exploit|...|-h>`: Show relevant the modules
- Search for a module: `search <keyword | cve:<year> platform:<os> | type:<auxiliary | exploit | ...>>`
- Select a module: `use <module full name | number from search>`
- Set values to variables: `set <variable_name> <value>`
	- For setting the value globally: `setg`
- Get list of all variables required to run a module: `show options`
- Based on target (x84 ot x64), select the required payload
- Attack the machine: `run` or `exploit`
- Get list of active sessions: `sessions`
- Banner grabbing (similar to `nc`): `connect -h`
- List of stored creds: `creds`
- Looted data: `loot`
- Services running on target host: `services`

### Workspaces
- Workspace related data is stored in MSF DB
- Check db_status: `db_status`
- Current workspace: `workspace` 
- Get details of target host: `hosts`
- Add : `workspace -a <name>`
- Change : `workspace <name>`
- Delete : `workspace -d <name>`
- Rename: `workspace -r <name>`

### Add plugins
- Download he plugin file and move it to `/usr/share/metasploit-framework/plugins`

## Automating Metasploit w/ Resource Scripts
- Similar to bash script but for metasploit
- Prepackaged scripts on Kali: `/usr/share/metasploit-framework/scripts/resources`
- Create a resource script
	- Create a new file `<name>.rc`
	- Add the commands in the exact sequence into it
		```
		use multi/handler
		set PAYLOAD windows/meterpreter/reverse_tcp
		set LHOST <your_ip>
		set LPORT <your_port>
		run
		```
	- Run as `msfconsole -r <name>.rc`
- Load a resource file from msfconsole
	```
	msfconsole> resource <resource_file_path>
	```
- Export already enter commands to an rc
	```
	msfconsole> makerc <name>.rc
	```
- 