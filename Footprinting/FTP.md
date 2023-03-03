# FTP

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
