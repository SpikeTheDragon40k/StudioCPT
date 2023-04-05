# Bind Shells
With a bind shell, the target system has a listener started and awaits a connection from a pentester's system (attack box).

## Challanges with Bind Shells
```
There would have to be a listener already started on the target.
If there is no listener started, we would need to find a way to make this happen.
Admins typically configure strict incoming firewall rules and NAT (with PAT implementation) on the edge of the network (public-facing), so we would need to be on the internal network already.
Operating system firewalls (on Windows & Linux) will likely block most incoming connections that aren't associated with trusted network-based applications.
```
# Reverse Shells
With a reverse shell, the attack box will have a listener running, and the target will need to initiate the connection.
