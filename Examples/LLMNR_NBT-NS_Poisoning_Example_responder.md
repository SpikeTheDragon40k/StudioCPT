# LLMNR/NBT-NS Poisoning Example - from Windows w/ Responder

1. A host attempts to connect to the print server at \\print01.inlanefreight.local, but accidentally types in \\printer01.inlanefreight.local.
1. The DNS server responds, stating that this host is unknown.
1. The host then broadcasts out to the entire local network asking if anyone knows the location of \\printer01.inlanefreight.local.
1. The attacker (us with Responder running) responds to the host stating that it is the \\printer01.inlanefreight.local that the host is looking for.
1. The host believes this reply and sends an authentication request to the attacker with a username and NTLMv2 password hash.
1. This hash can then be cracked offline or used in an SMB Relay attack if the right conditions exist.