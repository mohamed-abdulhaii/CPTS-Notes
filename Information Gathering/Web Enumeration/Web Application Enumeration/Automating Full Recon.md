
Recon Automation involves executing scripts and modular frameworks to automate repetitive information-gathering tasks—such as WHOIS lookups, DNS resolution, subdomain discovery, port scanning, and crawling—improving test coverage and speed.

- One tool to do all tasks 
there are many tools but we will use FinalRecon tool 

**FinalRecon**: Fast, Python-based all-in-one recon tool offering modular features for headers, SSL certificates, WHOIS, DNS enumeration, subdomain scraping, crawling, and directory brute-forcing

```
Command Syntax
./finalrecon.py --url http://target.com --headers --whois

Arguments (modules) : 
`--url URL`: Target URL address.
`--headers`: Pulls HTTP response headers.
`--sslinfo`: Extracts SSL/TLS certificate details and validity.
`--whois`: Performs automated WHOIS queries.
`--dns`: Enumerates over 40 DNS record types.
`--sub`: Discovers subdomains via public APIs (crt.sh, VirusTotal, ThreatMiner, Shodan).
`--crawl`: Crawls the target application for internal/external links, scripts, and media.
`--dir`: Executes directory and file brute-forcing using a specified wordlist.
`--full`: Runs all available reconnaissance modules sequentially against the target.
```