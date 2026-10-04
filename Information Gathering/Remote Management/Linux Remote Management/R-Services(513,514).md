# Cheat Sheet 


| Command                              | Description                                                                                                    |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| nmap -sV -p513,514 target            | Check for R-Services                                                                                           |
| nc target \<513 or 514>              | Banner grabbing                                                                                                |
| rsh target -l \<username> \<command> | try to perform a command on the target, you can try without a username if target trust all user or hosts--> ++ |
| rlogin target -l \<username>         | login If .rhostshas + + — you are in immediately with no password.                                             |

# R-Services

R-Services are a suite of remote access services developed for Unix systems. They provide remote shell access, file copying, and remote login capabilities. **WARNING**: R-Services are inherently insecure and should not be used in production environments.

**R-Service Components**

| **Service** | **Port** | **Description**        |
| ----------- | -------- | ---------------------- |
| **RSH**     | 514      | Remote shell execution |
| **RCP**     | 514      | Remote file copy       |
| **RLOGIN**  | 513      | Remote login           |

### R-Service Authentication
R-Services use host-based authentication through : 
- `.rhosts`: Per-user access control
- `/etc/hosts.equiv`: System-wide access control
- **Trusted hosts**: IP-based authentication
R-Services are old. They trust hosts instead of passwords. Two files control who is trusted:
- /etc/hosts.equiv— global trust list for all users
- ~/.rhosts— per-user trust list

**R-Service Security Issues** : 
1. **No Encryption**: All communication in plain text
2. **Weak Authentication**: Host-based authentication only
3. **Information Disclosure**: Verbose error messages
4. **Privilege Escalation**: Potential for root access
5. **Network Sniffing**: Credentials transmitted in clear text