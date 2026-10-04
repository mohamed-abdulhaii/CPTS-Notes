# Cheat Sheet

tools 

```bash
              SNMP
                │
        ┌───────┴────────┐
        │                │
   Find access       Get information
        │                │
   onesixtyone       snmpwalk / braa 
```

| Command                                                   | Description                                                                   |
| --------------------------------------------------------- | ----------------------------------------------------------------------------- |
| nmap -sU -p 161,162 -sV -sC -- script snmp-\*\<target>    | Basic UDP scan +SNMP scripts                                                  |
| snmpwalk -v1 -c public \<target>                          | version detection and enumerate OIDS.<br>-v 2c for the SNMPv2c                |
| snmpwalk -v3 \<target>                                    | version detection and enumerate OIDS for SNMPv3                               |
| onesixtyone \<target> public                              | version detection                                                             |
| snmpwalk -v1 -c \<public or private or manager> \<target> | Testing default community strings                                             |
| onesixtyone -c wordlist.txt \<target>                     | Testing community strings with wordlist                                       |
| nmap -sU -p 161 --script snmp-brute \<target>             | Testing community strings with NSE                                            |
| snmpwalk -v2c -c public \<target> 1.3.6.1.2.1.1.1.0       | enumerate Basic system info                                                   |
| snmpwalk -v2c -c public \<target> 1.3.6.1.2.1.25.4.2.1.2  | enumerate Running processes (system info)                                     |
| snmpwalk -v2c -c public \<target> 1.3.6.1.2.1.25.6.3.1.2  | enumerate Installed software(system info)                                     |
| snmpwalk -v2c -c public \<target> 1.3.6.1.2.1.2.2.1.2     | enumerate Network interfaces (Network Info)                                   |
| snmpwalk -v2c -c public \<target> 1.3.6.1.2.1.4.20.1.1    | enumerate IP addresses(Network Info)                                          |
| snmpwalk -v2c -c public \<target> 1.3.6.1.2.1.4.21.1      | enumerate Routing information(Network info)                                   |
| braa public@\<target>:.1.3.6.*                            | to brute-force the individual OIDs and enumerate the information behind them. |
| braa public@192.168.1.0\/24:.1.3.6.1.2.1.1.1.0            | mass scan                                                                     |
- [ ] Port Scanning and Version Detection
- [ ] Community String Testing
- [ ] Information Gathering : System Information, Network Information

Related Articles





# Overview 
Simple Network Management Protocol(SNMP), which lets network admins monitor and control servers, routers, and switches. Pentesters care about SNMP because it can leak network diagrams, passwords, and system settings, or even let you change device configurations remotely.
To access this service you must have a community string

Port 161 is used for standard SNMP requests.
Port 162 is used for SNMP Traps (automatic server alerts, real-time alert messages sent by network devices)
### Basic Information
- **Port**: UDP 161 (queries), UDP 162 (traps)
- **Protocol Type**: Network management protocol
- **Purpose**: Monitor and manage network devices
- **Security**: Varies by version (v1/v2c - weak, v3 - strong)

**Some Definitions you must know**
- **MIB (Management Information Base)**: This is a structured database on the server that holds all the device information.
- **OID (Object Identifier)**: This is a unique number chain (like1.3.6.1.2.1.1.1) that points to a specific piece of data inside the MIB.
- **An SNMP community string** is a plain-text password used to authenticate communication between a network management system and a managed device like a router or switch

How the authentication with community string work :
- Acts as a shared secret key attached to SNMP requests (such as GET or SET messages).
- The receiving device checks the string before sharing data or accepting configuration changes.
- Devices ignore requests that include an incorrect or missing string.

**SNMP Versions**
1. **SNMPv1**
    - Basic version
    - No encryption
    - Uses community strings
    - Still used in legacy systems
2. **SNMPv2c**
    - Enhanced performance
    - Community string based
    - No real security improvements
    - Widely deployed
3. **SNMPv3**
    - Strong authentication
    - Data encryption
    - Username/password based
    - Most secure version


How SNMP works with MID & OID :

OID is a number like this : 1.3.6.1.2.1.25 , but its a path in the tree
```bash
1
└── 3
    └── 6
        └── 1
            └── 2
                └── 1
                    ├── 1  → System
                    ├── 2  → Interfaces
                    ├── 4  → IP
                    └── 25 → Host Resources
```


Common MID areas

| OID              | info about         |
| ---------------- | ------------------ |
| `1.3.6.1.2.1.1`  | System information |
| `1.3.6.1.2.1.2`  | Network interfaces |
| `1.3.6.1.2.1.4`  | IP                 |
| `1.3.6.1.2.1.6`  | TCP                |
| `1.3.6.1.2.1.7`  | UDP                |
| `1.3.6.1.2.1.25` | Host Resources     |

**OID**                                            **Description**

1.3.6.1.2.1.1.1.0                          System Description
1.3.6.1.2.1.25.1.6.0                     System Processes
1.3.6.1.2.1.25.4.2.1.2                  Running Programs
1.3.6.1.2.1.25.4.2.1.4                  Processes Path
1.3.6.1.2.1.25.2.3.1.4                  Storage Units
1.3.6.1.2.1.25.6.3.1.2                  Software Name
1.3.6.1.4.1.77.1.2.25                   User Accounts