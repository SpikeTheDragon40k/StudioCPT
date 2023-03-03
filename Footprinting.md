# Enumeration
---
![Enumerate](/Images/enum-method3.png "Enumerate all the things")



| Layer | Description | Information Categories|
| ---------- |---------------------------|---------------------------|
| Internet Presence | Identification of internet presence and externally accessible infrastructure.| Domains, Subdomains, vHosts, ASN, Netblocks, IP Addresses, Cloud Instances, Security Measures |
| Gateway | Identify the possible security measures to protect the company's external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare |
| Accessible Services | Identify accessible interfaces and services that are hosted externally or internally. | Service Type, Functionality, Configuration, Port, Version, Interface |
| Processes | Identify the internal processes, sources, and destinations associated with the services. | PID, Processed Data, Tasks, Source, Destination |
| Privileges | Identification of the internal permissions and privileges to the accessible services. | Groups, Users, Permissions, Restrictions, Environment |
| OS Setup | Identification of the internal components and systems setup. | OS Type, Patch Level, Network config, OS Environment, Configuration files, sensitive private files |



![Labirinto](/Images/pentest-labyrinth.png "The squares represent the gaps/vulnerabilities.")

***
# Layers

## Layer No.1: Internet Presence
### Goal: Identify all possible target systems and interfaces that can be tested.
***
## Layer No.2: Gateway
### Goal: Understand what we are dealing with and what we have to watch out for.
***
## Layer No.3: Accessible Services
### Goal: This layer aims to understand the reason and functionality of the target system and gain the necessary knowledge to communicate with it and exploit it for our purposes effectively.
***
## Layer No.4: Processes
### Goal: Understand these factors and identify the dependencies between them.
***
## Layer No.5: Privileges
### Goal: It is crucial to identify these and understand what is and is not possible with these privileges.
***
## Layer No.6: OS Setup
### Goal: See how the administrators manage the systems and what sensitive internal information we can glean from them.

---

# Domain Information

- However, when passively gathering information, we can use third-party services to understand the company better. However, the first thing we should do is scrutinize the company's main website
## Online Presence
- The first point of presence on the Internet may be the SSL certificate from the company's main website that we can examine.
![Domain Info SSL](/Images/DomInfo-1.png "Domain Info SSL Certificate")

- Check Certificate Term Json
```bash
curl -s https://crt.sh/\?q\=$Domain\&output\=json | jq .
```
- Check Certificate Unique Subdomain
```bash
curl -s https://crt.sh/\?q\=$Domain\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```
---

# Staff Info

Job Post can reveal info on team infrastructure and makeup.
This, in turn, can lead to us identifying which technologies, programming languages, and even software applications are being used.
Employees can be identified on various business networks such as LinkedIn or Xing. Job postings from companies can also tell us a lot about their infrastructure and give us clues about what we should be looking for.

-Example:
```
Required Skills/Knowledge/Experience:

* 3-10+ years of experience on professional software development projects.

« An active US Government TS/SCI Security Clearance (current SSBI) or eligibility to obtain TS/SCI within nine months.
« Bachelor's degree in computer science/computer engineering with an engineering/math focus or another equivalent field of discipline.
« Experience with one or more object-oriented languages (e.g., Java, C#, C++).
« Experience with one or more scripting languages (e.g., Python, Ruby, PHP, Perl).
« Experience using SQL databases (e.g., PostgreSQL, MySQL, SQL Server, Oracle).
« Experience using ORM frameworks (e.g., SQLAIchemy, Hibernate, Entity Framework).
« Experience using Web frameworks (e.g., Flask, Django, Spring, ASP.NET MVC).
« Proficient with unit testing and test frameworks (e.g., pytest, JUnit, NUnit, xUnit).
« Service-Oriented Architecture (SOA)/microservices & RESTful API design/implementation.
« Familiar and comfortable with Agile Development Processes.
« Familiar and comfortable with Continuous Integration environments.
« Experience with version control systems (e.g., Git, SVN, Mercurial, Perforce).

Desired Skills/Knowledge/ Experience:

« CompTIA Security+ certification (or equivalent).
« Experience with Atlassian suite (Confluence, Jira, Bitbucket).
« Algorithm Development (e.g., Image Processing algorithms).
« Software security.
« Containerization and container orchestration (Docker, Kubernetes, etc.)
« Redis.
« NumPy.
```
***

# FTP & TFTP

TFTP Commands:
| Commands | Description |
| ---------- | ------------ |
| connect | Sets the remote host, and optionally the port, for file transfers. |
| get | Transfers a file or set of files from the remote host to the local host. |
| put | Transfers a file or set of files from the local host onto the remote host. |
| quit | Exits tftp. |
| status | Shows the current status of tftp, including the current transfer mode (ascii or binary), connection status, time-out value, and so on. |
| verbose | Turns verbose mode, which displays additional information during file transfer, on or off. |

Most used FTP Server on Linux-based ditro is vsFTPd the default config can be found in:
```bash
/etc/vsftpd.conf
```
The config ca be displayed with:
```bash
cat /etc/vsftpd.conf | grep -v "#"
```
| Setting | Description |
| ------- | ----------- |
| listen=NO	Run from inetd or as a standalone daemon? |
| listen_ipv6=YES | Listen on IPv6 ? |
| anonymous_enable=NO | Enable Anonymous access? |
| local_enable=YES | Allow local users to login? |
| dirmessage_enable=YES | Display active directory messages when users go into certain directories? |
| use_localtime=YES | Use local time? |
| xferlog_enable=YES | Activate logging of uploads/downloads? |
| connect_from_port_20=YES | Connect from port 20? |
| secure_chroot_dir=/var/run/vsftpd/empty | Name of an empty directory |
| pam_service_name=vsftpd | This string is the name of the PAM service vsftpd will use. |
| rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem | The last three options specify the location of the RSA certificate to use for SSL encrypted connections. |
| rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key |  |
| ssl_enable=NO | |

Also we can check the file where is the list of the denyed users:
```bash
cat /etc/ftpusers
```
## Dangerous Settings in FTP

| Setting | Description |
| ------- | ----------- |
| anonymous_enable=YES | Allowing anonymous login? |
| anon_upload_enable=YES | Allowing anonymous to upload files? |
| anon_mkdir_write_enable=YES | Allowing anonymous to create new directories? |
| no_anon_password=YES | Do not ask anonymous for password? |
| anon_root=/home/username/ftp | Directory for anonymous. |
| write_enable=YES | Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |

### Example of Anonymous FTP Login
```bash
XXXXXXXXXXXX@htb[/htb]$ ftp 10.129.14.136

Connected to 10.129.14.136.
220 "Welcome to the HTB Academy vsFTP service."
Name (10.129.14.136:cry0l1t3): anonymous

230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.


ftp> ls

200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-rw-r--    1 1002     1002      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Clients
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 1002     1002         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 1002     1002           41 Sep 14 16:45 Important Notes.txt
226 Directory send OK.

```

If this setting is turned on we can use recursive Listing "ls_recurse_enable=YES".

```bash
ftp> ls -R

---> PORT 10,10,14,4,222,149
200 PORT command successful. Consider using PASV.
---> LIST -R
150 Here comes the directory listing.
.:
-rw-rw-r--    1 ftp      ftp      8138592 Sep 14 16:54 Calender.pptx
drwxrwxr-x    2 ftp      ftp         4096 Sep 14 17:03 Clients
drwxrwxr-x    2 ftp      ftp         4096 Sep 14 16:50 Documents
drwxrwxr-x    2 ftp      ftp         4096 Sep 14 16:50 Employees
-rw-rw-r--    1 ftp      ftp           41 Sep 14 16:45 Important Notes.txt
-rw-------    1 ftp      ftp            0 Sep 15 14:57 testupload.txt

./Clients:
drwx------    2 ftp      ftp          4096 Sep 16 18:04 HackTheBox
drwxrwxrwx    2 ftp      ftp          4096 Sep 16 18:00 Inlanefreight

./Clients/HackTheBox:
-rw-r--r--    1 ftp      ftp         34872 Sep 16 18:04 appointments.xlsx
-rw-r--r--    1 ftp      ftp        498123 Sep 16 18:04 contract.docx
-rw-r--r--    1 ftp      ftp        478237 Sep 16 18:04 contract.pdf
-rw-r--r--    1 ftp      ftp           348 Sep 16 18:04 meetings.txt

./Clients/Inlanefreight:
-rw-r--r--    1 ftp      ftp         14211 Sep 16 18:00 appointments.xlsx
-rw-r--r--    1 ftp      ftp         37882 Sep 16 17:58 contract.docx
-rw-r--r--    1 ftp      ftp            89 Sep 16 17:58 meetings.txt
-rw-r--r--    1 ftp      ftp        483293 Sep 16 17:59 proposal.pptx

./Documents:
-rw-r--r--    1 ftp      ftp         23211 Sep 16 18:05 appointments-template.xlsx
-rw-r--r--    1 ftp      ftp         32521 Sep 16 18:05 contract-template.docx
-rw-r--r--    1 ftp      ftp        453312 Sep 16 18:05 contract-template.pdf

./Employees:
226 Directory send OK.
```

