
### Execution as a user

```bash
Adriano@htb[/htb]$ cat /etc/shadow

cat: /etc/shadow: Permission denied
```



### Execution as root

```bash
Adriano@htb[/htb]$ sudo cat /etc/shadow

root:<SNIP>:18395:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
<SNIP>
```

**Command** **Description**

`sudo` Execute command as a different user.

`su`     The `su` utility requests appropriate user credentials via PAM and switches to                 	 that user ID (the default user is the superuser). A shell is then executed.

`useradd` Creates a new user or update default new user information.

`userdel` Deletes a user account and related files.

`usermod` Modifies a user account.

`addgroup` Adds a group to the system.

`delgroup` Removes a group from the system.

`passwd` Changes user password.