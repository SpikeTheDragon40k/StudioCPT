# Passwd, Shadow & Opasswd in UNIX

## The Passwd File
> The /etc/passwd file contains information about every existing user on the system and can be read by all users and services. Each entry in the /etc/passwd file identifies a user on the system.

### Passwd Format

| Username	| :	x | :	1000 | :	1000 | :	Username,,, | :	/home/Username | :	/bin/bash |
| --------  | ---- |  -------- | -------- | ----------- | ----------- | --------- |  
| Login name | Password info | UID | GUID | Full name/comments | Home directory | Shell |

### Example Passwd File
```bash
root:x:0:0:root:/root:/bin/bash
```

> we find the value x in this field, which means that the passwords are stored in an encrypted form in the /etc/shadow file. However, it can also be that the /etc/passwd file is writeable by mistake. This would allow us to clear this field for the user root so that the password info field is empty. This will cause the system not to send a password prompt when a user tries to log in as root.
```bash
root::0:0:root:/root:/bin/bash
```
>Root without Password
```bash

[Username@parrot]─[~]$ head -n 1 /etc/passwd

root::0:0:root:/root:/bin/bash


[Username@parrot]─[~]$ su

[root@parrot]─[/home/Username]#
```

## The Shadow File

>Since reading the password hash values can put the entire system in danger, the file /etc/shadow was developed, which has a similar format to /etc/passwd but is only responsible for passwords and their management. It contains all the password information for the created users. For example, if there is no entry in the /etc/shadow file for a user in /etc/passwd, the user is considered invalid. The /etc/shadow file is also only readable by users who have administrator rights

### Shadow Format
| Username	| :	$6$wBRzy$...SNIP...x9cDWUxW1	| :	18937	| :	0	| :	99999	| :	7	| :	| :	| : |
| --------   |   ---------------------------     |  ----- |  ---- |  ------ |  ------ | ---- |  -------- |  ------ |
| Username		| Encrypted password		| Last PW change		| Min. PW age		| Max. PW age		| Warning period	| Inactivity period	| Expiration date	| Unused |

### Catting the Shadow File

```bash
[Username@parrot]─[~]$ sudo cat /etc/shadow

root:*:18747:0:99999:7:::
sys:!:18747:0:99999:7:::
...SNIP...
Username:$6$wBRzy$...SNIP...x9cDWUxW1:18937:0:99999:7:::
```
>The encrypted password also has a particular format by which we can also find out some information:
```
$<type>$<salt>$<hashed>
```
[Types of Algorithm](/Useful_Pages/Hash.md)

## The Opasswd File

> The file where old passwords are stored is the /etc/security/opasswd. Administrator/root permissions are also required to read the file if the permissions for this file have not been changed manually.

### Reading /etc/security/opasswd
```bash
Attacker@xxx[/xxx]$ sudo cat /etc/security/opasswd

Username:1000:2:$1$HjFAfYTG$qNDkF0zJ3v8ylCOrKB0kt0,$1$kcUjWZJX$E9uMSmiQeRh4pAAgzuvkq1
```


# - [Cracking Linux Credentials](/Useful_Pages/Cracking_Linux_Credentials.md)