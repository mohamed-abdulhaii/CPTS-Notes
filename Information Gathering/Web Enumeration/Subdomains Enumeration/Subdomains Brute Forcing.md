# Cheat Sheet 


| Command                                         | Description                                                                                                                      |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| dnsenum --enum \<target.com> -f wordlist.txt -r | enumerate subdomains of target by using wordlist. send query to DNS server of target to check for subdomain (means Active recon) |


# Overview 

Subdomains are extensions of a primary domain name (e.g., `admin.example.com` or `dev.example.com`) created to separate functionalities, environments, or services across an organization. Subdomains represent a massive portion of an organization's attack surface and frequently host high-value targets for penetration testers:

- **Development & Staging Environments**: Staging subdomains (e.g., `stage.target.com`, `dev.target.com`) often run unpatched code, possess debug flags, or lack corporate WAF protections. In other words, subdomains are underdevelopment or just for testing a service 

- **Hidden Portals & Admin Interfaces**: Exposed internal management panels (e.g., `admin.target.com`, `vpn.target.com`, `grafana.target.com`). In other words, subdomains must be unaccessable for clients 

- **Legacy & Outdated Applications**: Forgotten assets (e.g., `old-shop.target.com`) running obsolete, vulnerable software components.

- **Sensitive Data Exposure**: Accidentally indexed configuration files, backups, or internal API documentation.

### Subdomain Enumeration Methodologies

###### Active Subdomain Enumeration

1- **DNS Zone Transfer (AXFR)**
An administrative misconfiguration where a primary/secondary DNS server leaks its full zone table containing every registered record and subdomain to an unauthenticated requester.

2- **Brute-Force Enumeration** : Systematically sending queries using large wordlists of common subdomain names (e.g., `api`, `test`, `mail`, `portal`).

- **Tools**: `gobuster`, `ffuf`, `dnsenum`, `amass`, `pure-dns`.

###### Passive Subdomain Enumeration

1-  **Certificate Transparency (CT) Logs** : 
are public, append-only digital databases that record every SSL/TLS security certificate issued by Certificate Authorities (CAs). Subdomain names are extracted directly from the certificate's **Subject Alternative Name (SAN)** field.
- **Services**: `crt.sh`, `censys.io`.

2-  **Search Engine Operators (Google Dorking)** :
Utilizing targeted search filter operators to isolate subdomains indexed by search engine spiders.
- **Example Query**: `site:example.com -www`

3- **Passive Recon Aggregators** (OSINT Agents Search Engine)
Querying threat intelligence databases and historical DNS mapping datasets.
- **Tools & Services**: `VirusTotal`, `SecurityTrails`, `Shodan`, `subfinder`.
