
# Cheat Sheet

| Command                          | Description                                                                             |
| -------------------------------- | --------------------------------------------------------------------------------------- |
| curl -I https://<target.com>     | Banner Grabbing & HTTP Headers                                                          |
| wafw00f \<target.com>            | Identifying Web Application Firewalls                                                   |
| nikto -h \<target.com> -Tuning b | Scan web servers for dangerous files, outdated software versions, and misconfigurations |

# Overview

Fingerprinting is the process of extracting digital signatures and technical details about a target website's underlying stack—**including web server software, operating systems, Content Management Systems (CMS), scripting frameworks, and Web Application Firewalls (WAFs).** Identifying these exact technologies helps penetration testers prioritize targets, discover misconfigurations, and execute version-specific exploits.

CMS : It is a software application that lets you create, manage, edit, and publish digital content without needing to write code.
 
## Fingerprinting Techniques

1- **Banner Grabbing & HTTP Headers** : Retrieving HTTP headers using `curl` reveals detailed server components, custom headers, and redirection pathways.

2- WAF Identification (`wafw00f`) : Identifying Web Application Firewalls before active testing prevents unexpected rate-limiting or automated IP bans. So, we can avoid block from firewall


3- Web Technology Profiling Tools : Used to identify web technology, WAF and details about the web 
- **WhatWeb**: Fast CLI tool using signature databases to identify web technologies and CMS platforms.
- **Wappalyzer / BuiltWith**: Browser extensions and web services profiling frontend and backend stacks.
- **Nikto**: Web server vulnerability scanner capable of running targeted software identification modules.



 
