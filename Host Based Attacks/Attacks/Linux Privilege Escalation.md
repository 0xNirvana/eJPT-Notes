## Kernel Exploit
- Steps:
	- Identify the kernel vulnerability
	- Download, compile and transfer onto the target
- Tools: #tools/enum/linux/linux-exploit-suggester

### Exploitation
- Gain initial access on the machine
- Check the current user
- Downloa, transfer and run #tools/enum/linux/linux-exploit-suggester to the target machine
- It will give you some basic information about the target OS
- It will also provide a list of vulnerabilities that the target machine is vulnerable to
- Download the exploit and follow the instructions

## Misconfigured Cron Jobs
- Task scheduling to run applications, scripts and other commands
- `crontab` file contains all the cron jobs configured on the machine
- To exploit, we need to find and identify cron jobs scheduled by root or files being processed by cron jobs

## Exploitation
- Gain inital access to the target machine
- Run `crontab -l`
- Check permissions of the scripts/applications that are running as cron jobs and if you can modify them
- Give your current user higer privilege
	```
	printf '#!/bin/bash\necho "<current_user> ALL=NOPASSWD:ALL" >> /etc/sudoers' > <cron_related_file>
	```

## SUID Binaries
- Set Owner User ID
- Allows uniprivelged users to run specific binaries with elevated privileges
- Look for SUID binaries that are owned by root but the current user can also execute it
	```
	# find / -perm -4000 -user root
	```
- Run `strings` on the binary and look for other binaries that it is calling that you can modify
- Replace the called binary with `/bin/bash`
	```
	# rm <referenced file>
	# cp /bin/bash <referenced file>
	# ./<suid binary>
	```

## Dumping Credentials
- All acount details are stored in `/etc/passwd`
- All passwords are encrypted and stored in `/etc/shadow` file which can only be accessed by root account