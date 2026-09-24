
# Web Application Enumeration 

| Command                                     | Description                                                                                              |
| ------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| whois \<domain.com>                         | Extract public domain registration data, ownership details, and infrastructure info                      |
| curl -I https://<target.com>                | Banner Grabbing & HTTP Headers                                                                           |
| wafw00f \<target.com>                       | Identify and fingerprint Web Application Firewalls (WAF) protecting a website.                           |
| nikto -h \<target.com> -Tuning b            | Scan web servers for dangerous files, outdated software versions, and misconfigurations.                 |
| python3 ReconSpider.py http://\<target.com> | Crawls target websites to extract links, hidden directories, forms, and valuable structural information. |

**Automating Full Recon Tool**

```text
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


Google Dorks
```text
site:domain.com          # Limit to specific domain
inurl:login             # Find login pages
filetype:pdf            # Search for file types
intitle:"index of"      # Directory listing

-Finding Admin & Login Portals
site:target.com (inurl:login OR inurl:admin OR inurl:dashboard OR inurl:portal)

-Directory Listing & File Browsing
site:target.com intitle:"Index of /"

-Exposed Sensitive Documents
site:target.com (filetype:pdf OR filetype:doc OR filetype:xls OR filetype:xlsx)

-Leaked Configuration Files & Environment Keys
site:target.com (ext:xml OR ext:conf OR ext:cnf OR ext:reg OR ext:inf OR ext:env)

-Locating Database Backups
site:target.com (ext:sql OR ext:db OR ext:tar OR ext:zip OR ext:bak)
```


**Sensitive files and folders**

```text
/robots.txt
/.well-known/security.txt
/.well-known/change-password
/.well-known/openid-configuration
```