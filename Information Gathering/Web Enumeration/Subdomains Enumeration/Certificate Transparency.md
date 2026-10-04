
| Command                                                                                                          | Description                                            |
| ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| curl -s "https:\//crt.sh/?q=%.<example.com>&output=json" \| jq -r '.[].name_value' \| sed 's/\*\.//g' \| sort -u | enumerate subdomains for a given domain (from crt.sh ) |

# Overview

Certificate Transparency (CT) logs are public, append-only, tamper-proof ledgers that record every SSL/TLS certificate issued by trusted Certificate Authorities (CAs).

**Mechanics & Merkle Tree Integrity**
CT logs leverage Merkle Trees—a cryptographic binary tree structure—to store certificate hashes. Every root hash (Merkle Root) guarantees log data cannot be altered without altering the entire hash chain. 
- **What is a Merkle Tree?** Imagine a giant family tree, but instead of people, it is made of digital data (certificate hashes). At the very bottom are the individual pieces of data, and they combine step-by-step until they form a single "Root" at the very top.
- **Why is it secure?** If someone tries to secretly change even a single tiny detail in one certificate near the bottom, it changes the math for that branch. That change ripples all the way up and alters the **Merkle Root** at the very top.
- **The Main Benefit:** Because everyone can see the top Root hash, any tampering is immediately exposed. It acts like a digital tamper-proof seal for Certificate Transparency (CT) logs.