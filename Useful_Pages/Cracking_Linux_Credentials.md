# Cracking Linux Credentials

## Unshadow
```bash
Username@xxx[/xxx]$ sudo cp /etc/passwd /tmp/passwd.bak 
Username@xxx[/xxx]$ sudo cp /etc/shadow /tmp/shadow.bak 
Username@xxx[/xxx]$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```
## Hashcat - Cracking Unshadowed Hashes
```bash
Username@xxx[/xxx]$ hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```
## Hashcat - Cracking MD5 Hashes
```bash
Username@xxx[/xxx]$ cat md5-hashes.list

qNDkF0zJ3v8ylCOrKB0kt0
E9uMSmiQeRh4pAAgzuvkq1
```
```bash
Username@xxx[/xxx]$ hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```
