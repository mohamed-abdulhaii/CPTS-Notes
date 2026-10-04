# Cheat Sheet 

| Command                                                                                                       | Description                                    |
| ------------------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| host -t \<record> \<domain> \<nameserver>                                                                     | get a DNS record for a domain ,                |
| host -l \<doamin> /\<nameserver>                                                                              | get zone transfer, form a specific name server |
| dig \<record type> \<domain> @\<IP of name server >                                                           | dig tool , use to get records                  |
| dig ns <domain.tld> @\<IP of nameserver>                                                                      | get NS record, form a specific name server     |
| dig any <domain.tld> @\<nameserver>                                                                           | get all records                                |
| dig axfr <domain.tld> @\<nameserver>                                                                          | get zone transfer                              |
| dnsenum --dnsserver \<nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list \<domain.tld> | Subdomain brute forcing                        |

# Related Articles





# Overview

**DNS = Domain Name System**. It translates a domain name (like google.com) into an IP address (like 142.250.185.46). Think of it like a phone book for the internet. As a pentester, DNS can leak a lot of information about a target — subdomains, mail servers, internal hostnames.

**DNS is a distributed system**. This means the information is stored on thousands of DNS servers around the world. so we have types , and the info comes from the types

Common Records

| Record    | Question it answers                                 |
| --------- | --------------------------------------------------- |
| **A**     | What is the IPv4 address?                           |
| **AAAA**  | What is the IPv6 address?                           |
| **MX**    | Which server receives emails?                       |
| **NS**    | Which DNS servers manage this domain?               |
| **TXT**   | What additional text/security information exists?   |
| **CNAME** | Is this domain an alias for another domain?         |
| **PTR**   | Which domain belongs to this IP address?            |
| **SOA**   | Who manages this DNS zone and how is it configured? |

**A DNS Zone Transfer** : It is the process of copying all DNS records from one authoritative DNS server to another for synchronization. If it is misconfigured and accessible to everyone, it can leak the entire DNS database of a domain to an attacker.

all DNS have three types of config Files
1. Local File : tells DNS which ZONEs it manages , and where the Zone file for each zone
2. Zone File : contain all records and important info about the target
3. reverse name file : contain PTR records

DNS server have many options, But here are some Dangerous 

| Setting           | What it means for you                                           |
| ----------------- | --------------------------------------------------------------- |
| `allow-query`     | Who can ask DNS questions — if set to `any`, everyone can query |
| `allow-recursion` | If open = DNS amplification attack possible                     |
| `allow-transfer`  | If open = zone transfer works = you get everything              |

The goal of DNS enumeration to get information about the target, and create a big picture 
like this 

```bash
example.com
│
├── dev.example.com
│      ├── dev1.dev.example.com --------> IP : 10.10.15.1 , ports : 80 , 443 , 8080 , services : http , ftp 
│      ├── win1.dev.example.com
│      └── admin.dev.example.com
│
├── internal.example.com
│      ├── salary.internal.example.com
│      └── emp.internal.example.com
│
└── help.example.com
```