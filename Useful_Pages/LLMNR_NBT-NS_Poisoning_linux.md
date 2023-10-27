# LLMNR/NBT-NS Poisoning - from Linux

[Link-Local Multicast Name Resolution](https://datatracker.ietf.org/doc/html/rfc4795) (LLMNR) and [NetBIOS Name Service](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc940063(v=technet.10)?redirectedfrom=MSDN) (NBT-NS) are Microsoft Windows components that serve as alternate methods of host identification that can be used when DNS fails. If a machine attempts to resolve a host but DNS resolution fails, typically, the machine will try to ask all other machines on the local network for the correct host address via LLMNR. LLMNR is based upon the Domain Name System (DNS) format and allows hosts on the same local link to perform name resolution for other hosts. It uses port **5355** over UDP natively. If LLMNR fails, the NBT-NS will be used. NBT-NS identifies systems on a local network by their NetBIOS name. NBT-NS utilizes port **137** over UDP.

The kicker here is that when LLMNR/NBT-NS are used for name resolution, ANY host on the network can reply. This is where we come in with **Responder** to poison these requests. With network access, we can spoof an authoritative name resolution source ( in this case, a host that's supposed to belong in the network segment ) in the broadcast domain by responding to LLMNR and NBT-NS traffic as if they have an answer for the requesting host. This poisoning effort is done to get the victims to communicate with our system by pretending that our rogue system knows the location of the requested host. If the requested host requires name resolution or authentication actions, we can capture the NetNTLM hash and subject it to an offline brute force attack in an attempt to retrieve the cleartext password. The captured authentication request can also be relayed to access another host or used against a different protocol (such as LDAP) on the same host. LLMNR/NBNS spoofing combined with a lack of SMB signing can often lead to administrative access on hosts within a domain. SMB Relay attacks will be covered in a later module about Lateral Movement.

## [LLMNR/NBT-NS Poisoning Example - from Linux w/ Responder](/Examples/LLMNR_NBT-NS_Poisoning_Example_responder.md)

Tools that can be used to attempt LLMNR & NBT-NS poisoning:

| Tool | Description |
| ---- | ----------- |
| [Responder](https://github.com/lgandx/Responder) | Responder is a purpose-built tool to poison LLMNR, NBT-NS, and MDNS, with many different functions. |
| [Inveigh](https://github.com/Kevin-Robertson/Inveigh) | Inveigh is a cross-platform MITM platform that can be used for spoofing and poisoning attacks. |
| [Metasploit](https://www.metasploit.com/) | Metasploit has several built-in scanners and spoofing modules made to deal with poisoning attacks. |

Responder and Inveigh can be used to attack the following protocols:
- LLMNR
- DNS
- MDNS
- NBNS
- DHCP
- ICMP
- HTTP
- HTTPS
- SMB
- LDAP
- WebDAV
- Proxy Auth

Responder also has support for:
- MSSQL
- DCE-RPC
- FTP, POP3, IMAP, and SMTP auth


## [Responder In Action](/Tools_in_Use/Responder_inUse.md)
