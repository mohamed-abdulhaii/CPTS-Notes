# Cheat Sheet

| Command                                      | Description                                                                                                      |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| nmap -sV -p139,445 \<target\> --scripts smb* | scan SMB ports and try to get the version. And you can use useful scripts for this service                       |
| netexec smb \<target\> -u '' -p ''  --shares | enumerate all shares on the target by using null session (in other words , get the all names of exiting shares ) |
| netexec smb \<target\> -u '' -p ''  --users  | enumerate all users on the target by using null session                                                          |
| smbmap -H \<target\>                         | mapping for all shares                                                                                           |
| smbmap -H target -u user -p pass	            | mapping for all shares with Creds                                                                                |
| smbclient -N -L //target                     | listing shares, -L for list , -N for null session                                                                |
| smbclient //target_ip/sharename -U \<user>   | Connect to specific share                                                                                        |
| smbclient -N //target_ip/sharename           | Connect to specific share, with null session                                                                     |
| rpcclient -U "" <target\>                    | Interaction with the target using RPC                                                                            |
- [ ] scan port 139,445 for version , old versions have bug : EternalBlue
- [ ] enumerate shares on the target 
- [ ] search for sensitive files

# Related Articles 

smbmap tool : more commands. here [[TOOLS]]
rpcclient tool : a tool help us to find more info about shares and target. here [[TOOLS]]

# Overview 
**Server Massage Block (SMB)** is a client-server protocol that regulates access to files and entire directories and other network resources such as printers, routers, or interfaces released for the network. **SMB is for windows servers but linux use samba to implement SMB**

SMBv1 = CIF (Common Internet File System)

**NetBIOS** : its a old service or a program that help devices on the network to talk to each other and sharing files and resources. Old versions of SMB uses NetBIOS service to perform

**Workgroup** : a collection of devices sharing files without a Central Domain Controller 

- Access on shares is controlled using ACLs (Read, Write, Execute, Full Control) Permissions on shares 
```bash
Server
│
├── C:\HR
│      │
│      └── Shared as → \\Server\HR
│
└── ACL
       ├── Mohamed → Read
       ├── Ahmed   → Read + Write
       └── Admin   → Full Control

                 ▲
                 │ SMB (TCP 445)
                 │
Client ──────────┘
```


**SMB Configuration** can be global or each share have its configuration. Here are some configs 

| **Setting**                 | **Description**                                                     |
| --------------------------- | ------------------------------------------------------------------- |
| `browseable = yes`          | Allow listing available shares in the current share?                |
| `read only = no`            | Forbid the creation and modification of files?                      |
| `writable = yes`            | Allow users to create and modify files?                             |
| `guest ok = yes`            | Allow connecting to the service without using a password?           |
| `enable privileges = yes`   | Honor privileges assigned to specific SID?                          |
| `create mask = 0777`        | What permissions must be assigned to the newly created files?       |
| `directory mask = 0777`     | What permissions must be assigned to the newly created directories? |
| `logon script = script.sh`  | What script needs to be executed on the user's login?               |
| `magic script = script.sh`  | Which script should be executed when the script gets closed?        |
| `magic output = script.out` | Where the output of the magic script needs to be stored?            |
rule : if the setting make the employees comfort with the shares , it also make attacker comfort