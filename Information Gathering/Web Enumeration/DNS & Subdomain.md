
### **Certificate Transparency**

| Command                                                                                                          | Description                                            |
| ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| curl -s "https:\//crt.sh/?q=%.<example.com>&output=json" \| jq -r '.[].name_value' \| sed 's/\*\.//g' \| sort -u | enumerate subdomains for a given domain (from crt.sh ) |

### **DNS & Zone Transfer**

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

### **Subdomains Brute Forcing**

| Command                                         | Description                                                                                                                      |
| ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| dnsenum --enum \<target.com> -f wordlist.txt -r | enumerate subdomains of target by using wordlist. send query to DNS server of target to check for subdomain (means Active recon) |


### VHosts Fuzzing

| Command                                                                    | Description                                                                                                                                                                                         |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| gobuster vhost -u http:\//<target.com:port> -w \<wordlist> --append-domain | enumerate VHosts of the target, `--append-domain`: Appends the base domain (e.g., `.target.com`) to each word in the wordlist to construct the full hostname.                                       |
| ffuf -w \<wordlist> -u http:\//10.129.x.x -H "Host: FUZZ.\<target.com>"    | enumerate VHosts of the target, it sends **intentional, modified HTTP requests** directly to the target server and analyzes the responses to detect specific WAF behavior, signatures, and cookies. |