# Cheat Sheet 

| Commands                                                                | Description                                                                                                                      |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| ssh username@hostname                                                   | Login with Password authentication method                                                                                        |
| ssh -i private_key username@hostname                                    | Login with Public key authentication method                                                                                      |
| ssh -i certificate username@hostname                                    | Login with Certificate-based authentication method                                                                               |
| ssh -v \<user>@\<IP> -o PreferredAuthentications=password               | login, but force the service to use password                                                                                     |
| nc target 22 or telnet target 22                                        | Banner grabbing                                                                                                                  |
| nmap -p22 --script ssh-enum-users target                                | SSH user enumeration                                                                                                             |
| nmap -p22 --script ssh-enum-users --script-args userdb=users.txt target | User enumeration (if possible)                                                                                                   |
| hydra -l admin -P passwords.txt ssh://target                            | Brute force (if permitted)                                                                                                       |
| nmap -p22 --script ssh2-enum-algos target                               | SSH algorithm enumeration                                                                                                        |
| ./ssh-audit.py \<target>                                                | get more info, Look for[fail] lines — these are weak algorithms. Note the version from the banner. Search that version for CVEs. |
| use auxiliary/scanner/ssh/ssh_login                                     | brute force to login by using metasploit                                                                                         |


# Overview

Linux systems commonly use various remote management protocols for secure access and file transfer. These protocols enable remote administration, file synchronization, and system management across networks.

**Common protocols :** SSH (Secure Shell), Rsync and R-Services (RSH, RCP, RLOGIN)

# SSH (Secure Shell)

**SSH (Secure Shell)** is a network protocol that enables secure network communication and remote access to network services. ***It uses encryption*** to secure the communication channel between client and server.

- **Port 22**: Default SSH port
- **Authentication**: Public key, password, or certificate-based
- **Encryption**: AES, 3DES, ChaCha20-Poly1305
- **Integrity**: HMAC-SHA256, HMAC-SHA1
- **Key Exchange**: Diffie-Hellman, ECDH

**SSH Features**
- **Secure Remote Access**: Encrypted terminal sessions
- **File Transfer**: SCP and SFTP protocols
- **Port Forwarding**: Local and remote port forwarding
- **Tunneling**: Secure tunneling of other protocols
- **X11 Forwarding**: Remote GUI application access

SSH configuration file for client (your machine) : /etc/ssh/ssh_config
SSH configuration file for server : /etc/ssh/sshd_config



