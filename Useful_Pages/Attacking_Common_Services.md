# Attacker Common Services

## Server Message Block (SMB)

### Using CMD
> Windows CMD - DIR
```powershell
C:\xxx> dir \\192.168.220.129\Finance\

Volume in drive \\192.168.220.129\Finance has no label.
Volume Serial Number is ABCD-EFAA

Directory of \\192.168.220.129\Finance

02/23/2022  11:35 AM    <DIR>          Contracts
               0 File(s)          4,096 bytes
               1 Dir(s)  15,207,469,056 bytes free
```

> Windows CMD - Net Use
```powershell
C:\xxx> net use n: \\192.168.220.129\Finance

The command completed successfully.
```
> Windows CMD - Net Use with username and password to authenticate
```powershell
C:\xxx> net use n: \\192.168.220.129\Finance /user:plaintext Password123

The command completed successfully.
```
> Windows CMD - DIR - How many files the shared folder and its subdirectories contain.
```powershell
C:\xxx> dir n: /a-d /s /b | find /c ":\"

29302
```

| Syntax | Description |
| ------ | ----------- |
| dir | Application |
| n: | Directory or drive to search |
| /a-d | /a is the attribute and -d means not directories |
| /s | Displays files in a specified directory and all subdirectories |
| /b | Uses bare format (no heading information or summary) |

> Windows CMD - Findstr
```powershell
c:\xxx>findstr /s /i cred n:\*.*

n:\Contracts\private\secret.txt:file with all credentials
n:\Contracts\private\credentials.txt:admin:SecureCredentials!
```
### Using Powershell (PS)

> Windows PowerShell
```powershell
PS C:\xxx> Get-ChildItem \\192.168.220.129\Finance\

    Directory: \\192.168.220.129\Finance

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         2/23/2022   3:27 PM                Contracts
```
> New-PSDrive
```powershell
PS C:\xxx> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"

Name           Used (GB)     Free (GB) Provider      Root                                               CurrentLocation
----           ---------     --------- --------      ----                                               ---------------
N                                      FileSystem    \\192.168.220.129\Finance
```
> Windows PowerShell - PSCredential Object
```powershell
PS C:\xxx> $username = 'plaintext'
PS C:\xxx> $password = 'Password123'
PS C:\xxx> $secpassword = ConvertTo-SecureString $password -AsPlainText -Force
PS C:\xxx> $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
PS C:\xxx> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred

Name           Used (GB)     Free (GB) Provider      Root                                                              CurrentLocation
----           ---------     --------- --------      ----                                                              ---------------
N                                      FileSystem    \\192.168.220.129\Finance
```
> Windows PowerShell - GCI
```powershell
PS C:\xxx> N:
PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count

29302
```
```powershell
PS C:\xxx> Get-ChildItem -Recurse -Path N:\ -Include *cred* -File

    Directory: N:\Contracts\private

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         2/23/2022   4:36 PM             25 credentials.txt
```
> Windows PowerShell - Select-String
```powershell
PS C:\xxx> Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List

N:\Contracts\private\secret.txt:1:file with all credentials
N:\Contracts\private\credentials.txt:1:admin:SecureCredentials!
```

## Linux

> Linux - Mount
```bash
Attacker@xxx[/xxx]$ sudo mkdir /mnt/Finance
Attacker@xxx[/xxx]$ sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance
```

> Linux - Mount with credentials
```bash
Attacker@xxx[/xxx]$ mount -t cifs //192.168.220.129/Finance /mnt/Finance -o credentials=/path/credentialfile
```

> Credential File Structure
```
username=plaintext
password=Password123
domain=.
```

> Linux - Find
```bash
Attacker@xxx[/xxx]$ find /mnt/Finance/ -name *cred*

/mnt/Finance/Contracts/private/credentials.txt
```
```bash
Attacker@xxx[/xxx]$ grep -rn /mnt/Finance/ -ie cred

/mnt/Finance/Contracts/private/credentials.txt:1:admin:SecureCredentials!
/mnt/Finance/Contracts/private/secret.txt:1:file with all credentials
```

## Other Services

### Email
> Linux - Install Evolution
```bash
Attacker@xxx[/xxx]$ sudo apt-get install evolution
...SNIP...
```

[Video: Connecting to IMAP and SMTP using Evolution](https://www.youtube.com/watch?v=xelO2CiaSVs)


## Command Line Utilities

### MSSQL
> Linux - SQSH
```bash
Attacker@xxx[/xxx]$ sqsh -S 10.129.20.13 -U username -P Password123
```
> Windows - SQLCMD
```powershell
C:\xxx> sqlcmd -S 10.129.20.13 -U username -P Password123
```

> Linux - MySQL
```bash
Attacker@xxx[/xxx]$ mysql -u username -pPassword123 -h 10.129.20.13
```
> Windows - MySQL
```powershell
C:\xxx> mysql.exe -u username -pPassword123 -h 10.129.20.13
```

## GUI Application

> Install dbeaver
```bash
AdrianoInghihwg@htb[/htb]$ sudo dpkg -i dbeaver-<version>.deb
```

> Run dbeaver
```bash
AdrianoInghihwg@htb[/htb]$ dbeaver &
```
[Video - Connecting to MSSQL DB using dbeaver](https://www.youtube.com/watch?v=gU6iQP5rFMw)

[Video - Connecting to MySQL DB using dbeaver](https://www.youtube.com/watch?v=PeuWmz8S6G8)


# Tools

| SMB |	FTP |	Email	| Databases |
| --- | --- |   -----   | --------- |
| [smbclient](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html) | [ftp](https://linux.die.net/man/1/ftp) | [Thunderbird](https://www.thunderbird.net/en-US/) | [mssql-cli](https://github.com/dbcli/mssql-cli) |
| [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec) | [lftp](https://lftp.yar.ru/) | [Claws](https://www.claws-mail.org/) | [mycli](https://github.com/dbcli/mycli) |
| [SMBMap](https://github.com/ShawnDEvans/smbmap) | [ncftp](https://www.ncftp.com/) | [Geary](https://wiki.gnome.org/Apps/Geary) | [mssqlclient.py](https://github.com/fortra/impacket/blob/master/examples/mssqlclient.py) |
| [Impacket](https://github.com/SecureAuthCorp/impacket) | [filezilla](https://filezilla-project.org/) | [MailSpring](https://www.getmailspring.com/) | [dbeaver](https://github.com/dbeaver/dbeaver) |
| [psexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/psexec.py) | [crossftp](http://www.crossftp.com/) | [mutt](http://www.mutt.org/) | [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) |
| [smbexec.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbexec.py) | 	| 	[mailutils](https://mailutils.org/) | [SQL Server Management Studio or SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16) |
|  |  | [sendEmail](https://github.com/mogaal/sendemail) 	| |  
|  |  | [swaks](http://www.jetmore.org/john/code/swaks/)	| | 
|  |  | [sendmail](https://en.wikipedia.org/wiki/Sendmail) |  |	

