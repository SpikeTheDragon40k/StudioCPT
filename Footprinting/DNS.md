# Domain Name System (DNS)

![DNS](/Images/DNS1.png)

| DNS Record | Description |
| ---------- | ----------- |
| A | Returns an IPv4 address of the requested domain as a result. |
| AAAA | Returns an IPv6 address of the requested domain. |
| MX | Returns the responsible mail servers as a result. |
| NS | Returns the DNS servers (nameservers) of the domain. |
| TXT | This record can contain various information. The all-rounder can be used, e.g., to validate the Google Search Console or validate SSL | certificates. In addition, SPF and DMARC entries are set to validate mail traffic and protect it from spam. |
| CNAME | This record serves as an alias. If the domain www.hackthebox.eu should point to the same IP, and we create an A record for one and a CNAME record for the other. |
| PTR | The PTR record works the other way around (reverse lookup). It converts IP addresses into valid domain names. |
| SOA | Provides information about the corresponding DNS zone and email address of the administrative contact. |

## Dangerous Settings
[The Most Popular Types of DNS Attacks](https://securitytrails.com/blog/most-popular-types-dns-attacks)
| Option | Description |
| ---------- | ----------- |
| allow-query | Defines which hosts are allowed to send requests to the DNS server. |
| allow-recursion | Defines which hosts are allowed to send recursive requests to the DNS server. |
| allow-transfer | Defines which hosts are allowed to receive zone transfers from the DNS server. |
| zone-statistics | Collects statistical data of zones. |