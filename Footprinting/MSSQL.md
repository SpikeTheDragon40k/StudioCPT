# MSSQL

## Clients for MSSQL

| | | | | | 
| - | - | - | - | -| 
| [mssql-cli](https://learn.microsoft.com/en-us/sql/tools/mssql-cli?view=sql-server-ver15) | [SQL Server PowerShell](https://learn.microsoft.com/en-us/sql/powershell/sql-server-powershell?view=sql-server-ver15) | [HediSQL](https://www.heidisql.com/) | [SQLPro](https://www.macsqlclient.com/) | [Impacket's mssqlclient.py](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py) |

>Of the MSSQL clients listed above, pentester's may find Impacket's mssqlclient.py to be the most useful due to SecureAuthCorp's Impacket project being present on many pentesting distributions at install. To find if and where the client is located on our host, we can use the following command:

```bash
Attacker@xxx[/xxx]$ locate mssqlclient

/usr/bin/impacket-mssqlclient
/usr/share/doc/python3-impacket/examples/mssqlclient.py
```
## MSSQL Databases Structure

| Default System Database | Description |
| ----------------------- | ----------- |
| master | Tracks all system information for an SQL server instance |
| model | Template database that acts as a structure for every new database created. Any setting changed in the model database will be reflected in any new database created after changes to the model database |
| msdb | The SQL Server Agent uses this database to schedule jobs & alerts |
| tempdb | Stores temporary objects |
| resource | Read-only database containing system objects included with SQL server |
[Source](https://docs.microsoft.com/en-us/sql/relational-databases/databases/system-databases?view=sql-server-ver15)


## Dangerous Settings

Some Dangerous Settings:
1. MSSQL clients not using encryption to connect to the MSSQL server
1. The use of self-signed certificates when encryption is being used. It is possible to spoof self-signed certificates
1. The use of named pipes
1. Weak & default sa credentials. Admins may forget to disable this account


# Footprinting the Service

## NMAP MSSQL Script Scan
```bash
Attacker@xxx[/xxx]$ sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 <IP>

Starting Nmap 7.91 ( https://nmap.org ) at 2021-11-08 09:40 EST
Nmap scan report for <IP>
Host is up (0.15s latency).

PORT     STATE SERVICE  VERSION
1433/tcp open  ms-sql-s Microsoft SQL Server 2019 15.00.2000.00; RTM
| ms-sql-ntlm-info: 
|   Target_Name: SQL-01
|   NetBIOS_Domain_Name: SQL-01
|   NetBIOS_Computer_Name: SQL-01
|   DNS_Domain_Name: SQL-01
|   DNS_Computer_Name: SQL-01
|_  Product_Version: 10.0.17763

Host script results:
| ms-sql-dac: 
|_  Instance: MSSQLSERVER; DAC port: 1434 (connection failed)
| ms-sql-info: 
|   Windows server name: SQL-01
|   <IP>\MSSQLSERVER: 
|     Instance name: MSSQLSERVER
|     Version: 
|       name: Microsoft SQL Server 2019 RTM
|       number: 15.00.2000.00
|       Product: Microsoft SQL Server 2019
|       Service pack level: RTM
|       Post-SP patches applied: false
|     TCP port: 1433
|     Named pipe: \\<IP>\pipe\sql\query
|_    Clustered: false

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.52 seconds
```
## Metasploit auxillary scanner mssql_ping
```bash
msf6 auxiliary(scanner/mssql/mssql_ping) > set rhosts <IP>

rhosts => <IP>


msf6 auxiliary(scanner/mssql/mssql_ping) > run

[*] <IP>:       - SQL Server information for <IP>:
[+] <IP>:       -    ServerName      = SQL-01
[+] <IP>:       -    InstanceName    = MSSQLSERVER
[+] <IP>:       -    IsClustered     = No
[+] <IP>:       -    Version         = 15.0.2000.5
[+] <IP>:       -    tcp             = 1433
[+] <IP>:       -    np              = \\SQL-01\pipe\sql\query
[*] <IP>:       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```
# Connecting with Mssqlclient.py
```bash
Attacker@xxx[/xxx]$ python3 mssqlclient.py Administrator@<IP> -windows-auth

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

Password:
[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(SQL-01): Line 1: Changed database context to 'master'.
[*] INFO(SQL-01): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands

SQL> select name from sys.databases

name                                                                                                                               

--------------------------------------------------------------------------------------

master                                                                                                                             

tempdb                                                                                                                             

model                                                                                                                              

msdb                                                                                                                               

Transactions    
```



