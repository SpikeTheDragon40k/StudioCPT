# Enumeration
---
![Enumerate](/Images/enum-method3.png "Enumerate all the things")

---

| Layer | Description | Information Categories|
| ---------- |---------------------------|---------------------------|
| Internet Presence | Identification of internet presence and externally accessible infrastructure.| Domains, Subdomains, vHosts, ASN, Netblocks, IP Addresses, Cloud Instances, Security Measures |
| Gateway | Identify the possible security measures to protect the company's external and internal infrastructure. | Firewalls, DMZ, IPS/IDS, EDR, Proxies, NAC, Network Segmentation, VPN, Cloudflare |
| Accessible Services | Identify accessible interfaces and services that are hosted externally or internally. | Service Type, Functionality, Configuration, Port, Version, Interface |
| Processes | Identify the internal processes, sources, and destinations associated with the services. | PID, Processed Data, Tasks, Source, Destination |
| Privileges | Identification of the internal permissions and privileges to the accessible services. | Groups, Users, Permissions, Restrictions, Environment |
| OS Setup | Identification of the internal components and systems setup. | OS Type, Patch Level, Network config, OS Environment, Configuration files, sensitive private files |

---

![Labirinto](/Images/pentest-labyrinth.png "The squares represent the gaps/vulnerabilities.")

---
# Layers

## Layer No.1: Internet Presence
### Goal: Identify all possible target systems and interfaces that can be tested.
***
## Layer No.2: Gateway
### Goal: Understand what we are dealing with and what we have to watch out for.
***
## Layer No.3: Accessible Services
### Goal: This layer aims to understand the reason and functionality of the target system and gain the necessary knowledge to communicate with it and exploit it for our purposes effectively.
***
## Layer No.4: Processes
### Goal: Understand these factors and identify the dependencies between them.
***
## Layer No.5: Privileges
### Goal: It is crucial to identify these and understand what is and is not possible with these privileges.
***
## Layer No.6: OS Setup
### Goal: See how the administrators manage the systems and what sensitive internal information we can glean from them.

---