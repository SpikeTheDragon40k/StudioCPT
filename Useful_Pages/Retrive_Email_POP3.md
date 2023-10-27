# Retrieve Email from a POP3 Server using the Command Line

### 1: Open a connection from your computer to a POP3 mail server
```bash
telnet pop.domain.ext 110
Trying ???.???.???.???...
Connected to pop.domain.ext.
Escape character is '^]'.
+OK ready
```
### 2: Type your Username
```bash
> USER username
+OK Password required for UserName.
```

### 3: Type your Password
```bash
> PASS password
+OK username has ? visible messages (? hidden) in ????? octets.
```

## POP3 Commands with Description

| Command | Description | Example |
| ------- | ----------- | ------- |
| USER [username] | 1st login command | USER Stan +OK Please enter a password |
| PASS [password] |	2nd login command |	PASS SeCrEt +OK valid logon |
| QUIT |	Logs out and saves any changes |	QUIT +OK Bye-bye. |
| STAT |	Returns total number of messages and total size |	STAT +OK 2 320 |
| LIST |	Lists all messages | LIST +OK 2 messages (320 octets) 1 120 2 200 … LIST 2 +OK 2 200 |
| RETR [message] |	Retrieves the whole message |	RETR 1 +OK 120 octets follow. *** |
| DELE [message] |	Deletes the specified message |	DELE 2 +OK message deleted |
| NOOP |	The POP3 server does nothing, it merely replies with a positive response. |	NOOP +OK |
| RSET | Undelete the message if any marked for deletion | RSET +OK maildrop has 2 messages (320 octets) |
| TOP [message] [number] |	Returns the headers and number of lines from the message |	TOP 1 10 +OK *** |
