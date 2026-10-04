
# Overview

Web Application Information Gathering is a specialized phase of reconnaissance that focuses on web applications and their underlying technologies. Unlike infrastructure enumeration, this phase targets the application layer to identify technologies, frameworks, hidden files, parameters, and potential attack vectors.

**Web Reconnaissance in Two Sections :**

1- Subdomains & VHosts Enumeration
- Passive Discovery : whois, Certificate Transparency and search engine dorks.
- Active Enumeration : DNS enumeration & zone transfer (`dig`,`dnsenum`), Bruteforcing & Fuzzing(`gobuster`,`ffuf`) 

2- Web Application Enumeration : 
- **Tech Stack Fingerprinting**: Identifying backend frameworks, Content Management Systems(CMS) versions, and server configurations using tools like `wafw00f`, `nikto`, or browser extensions.

- **Crawling (Spidering) --- Directory & Content Discovery** : Using tools to follow every link on the site automatically, scraping for emails, scripts, comments, and historical endpoints. And checking `robots.txt`, and looking into `.well-known` directories.

-------------------------------------------------




