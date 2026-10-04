# Cheat Sheet 

| Commad                                                          | Description               |
| --------------------------------------------------------------- | ------------------------- |
| sudo nmap -sV -p 873 \<IP>                                      | Service Detection         |
| rsync target::<br>rsync rsync://target/                         | List available modules    |
| rsync target::module_name/<br>rsync rsync://target/module_name/ | Enumerate module contents |
| rsync -av rsync:// \<IP>/ \<share name> ./ \<local folder>/     | Download files            |

# Overview

Rsync is a utility for efficiently transferring and synchronizing files between computers. It uses the rsync protocol to transfer only the differences between files, making it bandwidth-efficient.
It is a file copy tool. The dangerous thing — sometimes it allows connections with NO authentication. You can list and download files freely. can be configured to use SSH for secure file transfers by piggybacking on top of an established SSH server connection

- **Port 873**: Default rsync daemon port
- **Protocol**: Custom rsync protocol over TCP
- **Efficiency**: Delta-sync algorithm (only transfers changes)
- **Authentication**: Module-based access control
- **Encryption**: Can tunnel through SSH