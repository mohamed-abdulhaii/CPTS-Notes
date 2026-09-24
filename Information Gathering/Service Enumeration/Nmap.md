
# Syntax

```
nmap <scan types> <options> <target>
```

## Scanning options

```
# HOST DISCOVERY
-sn                 # Host discovery only — no port scan
-Pn                 # Skip host discovery — treat host as online
-PE                 # ICMP Echo Request for host discovery
--disable-arp-ping  # Disable ARP-based host discovery


# PORT SCANNING
-p-                 # Scan all 65,535 ports
-p21-443            # Scan ports 21 through 443
-p25,110,143        # Scan specific ports: 25, 110, 143
-F                  # Fast scan — top 100 most common ports


# SCAN TYPES
-sS                 # TCP SYN scan
-sA                 # TCP ACK scan
-sU                 # UDP scan


# SERVICE / OS ENUMERATION
-sV                 # Detect service versions
-sC                 # Run default NSE scripts
--script <script>   # Run specific NSE scripts
-O                  # OS detection
-A                  # OS + version + default scripts + traceroute


# SCAN / TRAFFIC
-D RND:5            # Scan using 5 random decoy IPs
-S <IP>             # Spoof the source IP address
--packet-trace      # Show packets sent and received

# Performance options
-T <0-5>            # Specifies the specific timing template
-v/-vv              # Displays verbose output during the scan
--min-rate          # Sets the number of packets that will be sent simultaneously

```

