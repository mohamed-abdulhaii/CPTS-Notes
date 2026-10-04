# Cheat Sheet 


| Command                               | Description                                                                                                                       |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| dig \<domain.com> A                   | Retrieves the target IPv4 address.                                                                                                |
| dig \<domain.com> AAAA                | Retrieves IPv6 addresses.                                                                                                         |
| dig \<domain.com> MX                  | Retrieves mail exchange server records.                                                                                           |
| dig \<domain.com> NS                  | Queries authoritative name servers.                                                                                               |
| dig \<domain.com> TXT                 | Extracts text records (SPF, verification strings, etc.).                                                                          |
| dig \<domain.com> CNAME               | Queries canonical alias hostnames.                                                                                                |
| dig \<domain.com> SOA                 | Retrieves Start of Authority details. info about zone                                                                             |
| dig @1.1.1.1 domain.com               | Sends the query to a specific DNS server (e.g., Cloudflare 1.1.1.1).                                                              |
| dig +trace domain.com                 | Traces the full DNS resolution path from root servers down to authoritative name servers.                                         |
| dig -x \<IP>                          | Performs a reverse DNS lookup to find the associated hostname for an IP.                                                          |
| dig +short domain.com                 | Outputs only the raw answer string (clean, concise output).                                                                       |
| dig domain.com ANY                    | Retrieves all available DNS records for the domain (Note: Many DNS servers ignore `ANY` queries to reduce load and prevent abuse) |
| dig axfr @\<nameServer> \<domain.com> | Queries zone transfer from the primary or secondary DNS server of the target                                                      |


# Related Articles

**DNS Service & Enumeration** : [link](obsidian://open?vault=pentest%20(CPTS)&file=Information%20Gathering%2FService%20Enumeration%2FDNS%20(53)), Know more about the DNS

# Overview

DNS Reconnaissance is the process of querying DNS servers to gather information about a target's subdomains, mail routing, name servers, and internal IP infrastructure. Specialized CLI utilities like `dig` provide detailed responses that help map out an organization's attack surface.

Local Name Resolution: The Hosts File
The hosts file bypasses standard network DNS lookups, serving as a static local lookup table. And the machine go to it first before the query from DNS 
- **Paths**:
    - Windows: `C:\Windows\System32\drivers\etc\hosts`
    - Linux/MacOS: `/etc/hosts`
example for Host file
```
127.0.0.1       localhost
192.168.1.10    devserver.local
0.0.0.0         unwanted-site.com
```

##### Important Points to Look for 

- **Asset Discovery**: CNAME records can point to forgotten legacy servers or abandoned cloud infrastructure, leading to Subdomain Takeover vulnerabilities.
- **Infrastructure Mapping**: Identifying A, NS, and MX records helps layout internal network subnets, load balancers, firewalls, and mail gateway locations.
- **Technology & Fingerprinting Leakage**: TXT records frequently reveal third-party services integrated with the organization (e.g., Google Site Verification, 1Password, Zendesk, Salesforce).

##### The Structure of Dig Output : 

**Header** : Contains `opcode`, status (e.g., `NOERROR`), unique query ID, and flags like `qr` (Query Response), `rd` (Recursion Desired), and `ad` (Authentic Data).
**this section indicate the query is success or not**

**Question Section** : Details the queried domain and requested record type.

**Answer Section** : Displays the returned record, TTL (Time-To-Live), record class (`IN`), type, and target IP address.

**Footer** : Indicates total execution time in milliseconds, the local/upstream DNS server IP, timestamp, and payload size.


### Zone Transfer 

DNS Zone Transfer is the process that happen between the primary and secondary DNS servers to exchange all records to ensure high availability and load distribution. When left unsecured without proper Access Control Lists (ACLs), unauthenticated third parties can execute full zone transfer requests (AXFR) to dump every internal/external record and subdomain defined within the DNS zone.

##### The process occur in steps : 
1- **AXFR Request**: The secondary name server sends an `AXFR` query to the primary server over TCP port 53.
2- **SOA Record Exchange**: The primary server responds with its Start of Authority (SOA) record to check serial numbers.
3- **DNS Records Transmission**: The primary server streams all resource records (`A`, `AAAA`, `MX`, `CNAME`, `TXT`, `PTR`, `SRV`) sequentially.
4- **Completion & ACK**: Primary server signals the end of the transfer; the secondary server responds with an ACK packet.

##### What happen if we can get Zone Transfer 
- **Complete Subdomain Mapping**: Instantly exposes unlinked environments like staging servers (`stage.internal.com`), admin consoles, and backup gateways.
- **Internal Network Topology**: Maps IP distribution ranges, internal active directory controllers, and external interface targets.
- **Service Enumeration**: Exposes active internal services through `SRV` and `TXT` records (e.g., SIP, LDAP, Kerberos endpoints).
