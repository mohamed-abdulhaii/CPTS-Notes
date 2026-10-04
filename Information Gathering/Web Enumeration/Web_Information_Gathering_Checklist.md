# Web_Information_Gathering_Checklist (1)

---

Work top to bottom. Passive first (no footprint on the target), then active (touches the target directly).

- [ ]  **1. WHOIS lookup** — get registrar, registrant, name servers, creation/expiry dates
- [ ]  **2. Passive DNS recon** — `dig`, `host`, `nslookup` on the main domain
- [ ]  **3. Certificate Transparency search** — crt.sh / Censys for subdomains, no interaction with target
- [ ]  **4. Wayback Machine** — check historical snapshots for old pages/subdomains
- [ ]  **5. Google Dorking** — search operators for exposed files, logins, configs
- [ ]  **6. robots.txt + .well-known/** — check for disallowed paths and metadata endpoints
- [ ]  **7. Active DNS zone transfer attempt** — `dig axfr` (always worth trying)
- [ ]  **8. Subdomain brute-forcing** — dnsenum with a wordlist
- [ ]  **9. VHost fuzzing** — gobuster/ffuf on discovered IPs
- [ ]  **10. Fingerprinting** — banner grab (curl), WAF check (wafw00f), Nikto scan
- [ ]  **11. Crawling/Spidering** — Scrapy/ReconSpider to map the site
- [ ]  **12. Consolidate with an automation framework** — FinalRecon to fill gaps and cross-check manual findings
- [ ]  **13. Document everything** — every subdomain, IP, tech version, exposed file, and email found

---

## What you should do and note 

### WHOIS

- [ ]  Run WHOIS on primary domain
- [ ]  Note registrar, creation/expiry date, registrant org, name servers
- [ ]  Flag anything suspicious (recent registration, hidden registrant, bulletproof host) if investigating phishing/malware

---

### DNS — Manual Lookup Tools

- [ ]  Pull A, AAAA, MX, NS, TXT, SOA records
- [ ]  Decode TXT records for SPF/third-party service leaks
- [ ]  Reverse lookup any discovered IPs

---

### DNS — Zone Transfer

- [ ]  Identify name servers first (`dig ns domain.com`)
- [ ]  Attempt AXFR against each name server found
- [ ]  If successful, extract every subdomain + IP in one shot

---

### DNS — Automated Enumeration / Subdomain Brute-Forcing

- [ ]  Choose a wordlist (general-purpose or targeted)
- [ ]  Run dnsenum for brute-force

---

### Certificate Transparency (Passive Subdomain Discovery)

- [ ]  Query crt.sh for the target domain
- [ ]  Filter/sort unique subdomains
- [ ]  Cross-reference against brute-force results — CT logs often reveal subdomains wordlists miss

---

### Virtual Host (VHost) Discovery

- [ ]  Get the target’s IP address first
- [ ]  Run vhost fuzzing with a subdomain wordlist
- [ ]  Add discovered vhosts to the report

---

### Fingerprinting

- [ ]  Banner-grab every redirect hop, not just the first response
- [ ]  Check for a WAF before running further active scans
- [ ]  Run Nikto fingerprinting mode
- [ ]  Note exact software + version numbers for later CVE lookup

---

### Crawling / Spidering

- [ ]  Run a crawler against the target
- [ ]  Review `results.json` (or equivalent) for emails, external files, JS files, comments
- [ ]  Manually visit any interesting directories the crawler surfaces (e.g. `/files/`)

---

### robots.txt / .well-known

- [ ]  Fetch robots.txt, note every Disallow path
- [ ]  Manually visit each disallowed path
- [ ]  Check `.well-known/` for openid-configuration, security.txt, assetlinks.json

---

### Search Engine Discovery / Google Dorking

| Operator | Comment |
| --- | --- |
| `site:` | Scope search to one domain |
| `inurl:` | Term in the URL |
| `filetype:` | Search by file extension |
| `intitle:` | Term in page title |
| `intext:` | Term in body text |
| `-` / `NOT` | Exclude a term |
| `" "` | Exact phrase |

```
site:domain.com filetype:pdf
site:domain.com inurl:login
site:domain.com (ext:conf OR ext:cnf)
```

- [ ]  Search for exposed files (`filetype:pdf`, `filetype:sql`, `filetype:xls`)
- [ ]  Search for login/admin pages (`inurl:login`, `inurl:admin`)
- [ ]  Search for config/backup files (`inurl:config`, `inurl:backup`)
- [ ]  Check Google Hacking Database (GHDB) for more targeted dorks

---

### Web Archives

| Tool | Comment |
| --- | --- |
| Wayback Machine | Historical website snapshots — fully passive, zero detection risk |
- [ ]  Enter target URL into web.archive.org
- [ ]  Check earliest capture for original site structure
- [ ]  Compare snapshots over time for removed pages/subdomains/tech changes

---

### All-in-One Automation Frameworks

```bash
./finalrecon.py --full --url http://domain.com

```

- [ ]  Run one all-in-one framework as a final sweep to catch anything missed manually
- [ ]  Use theHarvester specifically for email/employee harvesting
- [ ]  Cross-check automated output against your manual findings — don’t trust either blindly

---

## Final Documentation Checklist

- [ ]  All discovered subdomains + their IPs logged
- [ ]  All discovered technologies + exact versions logged
- [ ]  WAF presence and type noted
- [ ]  All exposed files (PDFs, backups, configs) saved/logged
- [ ]  All employee names/emails logged
- [ ]  robots.txt disallowed paths manually checked
- [ ]  Screenshot or save evidence for anything sensitive found
