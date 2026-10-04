# Cheat Sheet 

| Command                                                                    | Description                                                                                                                                                                                         |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| gobuster vhost -u http:\//<target.com:port> -w \<wordlist> --append-domain | enumerate VHosts of the target, `--append-domain`: Appends the base domain (e.g., `.target.com`) to each word in the wordlist to construct the full hostname.                                       |
| ffuf -w \<wordlist> -u http:\//10.129.x.x -H "Host: FUZZ.\<target.com>"    | enumerate VHosts of the target, it sends **intentional, modified HTTP requests** directly to the target server and analyzes the responses to detect specific WAF behavior, signatures, and cookies. |

# Overview

Virtual Hosting allows a single web server (like Apache, Nginx, or IIS) to host multiple websites or applications on one server and IP address. The web server uses the **HTTP Host Header** sent by the browser to route incoming traffic to the correct website directory.

**Subdomains VS VHosts:**
- **Subdomain**: A DNS-level distinction (e.g., `dev.example.com`) mapped to an IP address via A/AAAA records in public or internal DNS servers.
- **Virtual Host (VHost)**: A web-server configuration level distinction. A server can host non-public VHosts that do **not** have public DNS records. You can only access them by overriding the local `/etc/hosts` file or manipulating the HTTP `Host` header.

### Types of Virtual Hosting
1- **Name-Based Virtual Hosting** (Most Common): Uses the HTTP `Host` header to route traffic for multiple domains on a single IP address.
2- **IP-Based Virtual Hosting**: Assigns a unique IP address to each website hosted on the server.
3- **Port-Based Virtual Hosting**: Runs different websites on different TCP ports (e.g., Port 80, Port 8080) on the same IP address.


### VHost Fuzzing 
When a target uses Name-Based Virtual Hosting, we send HTTP requests to the target IP address while fuzzing/brute-forcing different hostnames in the `Host` header.
(عملية الـ VHost Fuzzing بتعتمد على إرسال طلبات HTTP لـ IP السيرفر مع تغيير قيمة الـ Host Header في كل طلب لتخمين الـ VHosts الخفية.)

**Key Tools** : 
- gobuster: Multi-purpose tool with a dedicated vhost mode.
أداة سريعة ومشهورة لتخمين الـ VHosts والـ Directories.

- ffuf: Extremely fast and flexible web fuzzer often used to fuzz the Host header.
فوزر سريع جداً ومرن للتخمين على الـ Host Header.

- feroxbuster: Fast Rust-based recursive scanner supporting vhost discovery.
أداة مكتوبة بلغة Rust سريعة في التخمين والتصفية