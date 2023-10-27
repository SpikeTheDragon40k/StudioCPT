# Credential Hunting

Example is MySQL database credentials within WordPress configuration files:
```bash
Student@NIX02:~$ cat wp-config.php | grep 'DB_USER\|DB_PASSWORD'

define( 'DB_USER', 'wordpressuser' );
define( 'DB_PASSWORD', 'WPadmin123!' );
```

The spool or mail directories, if accessible, may also contain valuable information or even credentials. It is common to find credentials stored in files in the web root (i.e. MySQL connection strings, WordPress configuration files).
```bash
Student@NIX02:~$  find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null

/etc/ssh/ssh_config
/etc/ssh/sshd_config
/etc/python3/debian_config
/etc/kbd/config
/etc/manpath.config
/boot/config-4.4.0-116-generic
/boot/grub/i386-pc/configfile.mod
/sys/devices/pci0000:00/0000:00:00.0/config
/sys/devices/pci0000:00/0000:00:01.0/config
<SNIP>
```
## SSH Key Hunting

It is also useful to search around the system for accessible SSH private keys. We may locate a private key for another, more privileged, user that we can use to connect back to the box with additional privileges. We may also sometimes find SSH keys that can be used to access other hosts in the environment. Whenever finding SSH keys check the known_hosts file to find targets. This file contains a list of public keys for all the hosts which the user has connected to in the past and may be useful for lateral movement or to find data on a remote host that can be used to perform privilege escalation on our target.
```bash
Student@NIX02:~$  ls ~/.ssh

id_rsa  id_rsa.pub  known_hosts
```
