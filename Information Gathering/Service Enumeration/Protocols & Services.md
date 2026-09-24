
# FTP(21)
| command                                                            | Description                                                               |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------- |
| sudo nmap -sV -p21 -sC -A \<target\>                               | Run an aggressive scan , try to get the version , use the default scripts |
| sudo nmap -p21 --script ftp-* \<target>                            | Run FTP-specific scripts                                                  |
| nc -nv <FQDN/IP> 21                                                | Interact with the FTP service , and grab the banner                       |
| telnet <FQDN/IP> 21                                                | Interact with the FTP service , and grab the banner                       |
| openssl s_client -connect <FQDN/IP>:21 -starttls ftp               | Interact with the FTP service on the target using encrypted connection.   |
| use auxiliary/scanner/ftp/(ftp_version or anonymous) in metasploit | use modules in metasploit , gives more info                               |

**FTP Commands**

```text
ftp <target> -----> interact with ftp service 
debug ------------> make the server show us more information             
trace ------------> make the server show us more information
ls    ------------> list the content
ls -R ------------> recurse list
get <file> -------> download a file
put <file> -------> upload a file 
wget -m --no-passive ftp://anonymous:anonymous@<target> -------> download all the files and folders we have access to at once , this can cause alarms'
```


# SMB(139, 445)

| Command                                      | Description                                                                                                      |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| nmap -sV -p139,445 \<target\> --scripts smb* | scan SMB ports and try to get the version. And you can use useful scripts for this service                       |
| netexec smb \<target\> -u '' -p ''  --shares | enumerate all shares on the target by using null session (in other words , get the all names of exiting shares ) |
| netexec smb \<target\> -u '' -p ''  --users  | enumerate all users on the target by using null session                                                          |
| smbmap -H \<target\>                         | mapping for all shares                                                                                           |
| smbmap -H target -u user -p pass             | mapping for all shares with Creds                                                                                |
| smbclient -N -L //target                     | listing shares, -L for list , -N for null session                                                                |
| smbclient //target_ip/sharename -U \<user>   | Connect to specific share                                                                                        |
| smbclient -N //target_ip/sharename           | Connect to specific share, with null session                                                                     |
| rpcclient -U "" <target\>                    | Interaction with the target using RPC                                                                            |

**Rpc Commands**

| **Query**                 | **Description**                                                    |
| ------------------------- | ------------------------------------------------------------------ |
| `srvinfo`                 | Server information.                                                |
| `enumdomains`             | Enumerate all domains that are deployed in the network.            |
| `querydominfo`            | Provides domain, server, and user information of deployed domains. |
| `netshareenumall`         | Enumerates all available shares.                                   |
| `netsharegetinfo <share>` | Provides information about a specific share.                       |
| `enumdomusers`            | Enumerates all domain users.                                       |
| `queryuser <RID>`         | Provides information about a specific user.                        |
| `querygroup <RID>`        | Provides information about a specific user                         |


# NFS(2049)
| Command                                                   | Description                                                           |
| --------------------------------------------------------- | --------------------------------------------------------------------- |
| sudo nmap \<IP> -p111,2049 -sV -sC                        | scan with NSE, mostly get all rpc ports and services                  |
| showmount -e \<IP>                                        | Show available NFS shares                                             |
| sudo mount -t nfs \<IP>:/\<share> ./target-NFS/ -o nolock | Mount the specific NFS share to ./target-NFS (in your local machine ) |
| sudo umount ./target-NFS                                  | Unmount the specific NFS share                                        |
| nmap --script nfs* -p111,2049 \<target>                   | use NSE to get more info about RPC service and shares in NFS service  |
| rpcinfo -p \<target>                                      | Get RPC service information                                           |

# DNS (53)

| Command                                                                                                       | Description                            |
| ------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| dig \<record type> \<domain> @\<IP of name server >                                                           | Syntax of used tool                    |
| dig any <domain.tld> @\<nameserver>                                                                           | ANY request to the specific nameserver |
| dig axfr <domain.tld> @\<nameserver>                                                                          | Zone Transfer request                  |
| dnsenum --dnsserver \<nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list \<domain.tld> | Subdomain brute forcing                |

# SMTP (25, 587, 465)
| Command                                           | Description                                                                                                                                      |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| telnet \<target> 25                               | try to grab the banner, it contain information                                                                                                   |
| nc -nv \<IP> 25                                   | also grab the banner                                                                                                                             |
| nmap -sC -sV -p25 -v target                       | use default scripts to enumerate the service and the default scripts contains smtp-commands scripts which lists all possible valid smtp commands |
| sudo nmap -p25 --script smtp-open-relay -v target | check if the mail server is a open relay by using 16 different methods                                                                           |
| smtp-user-enum -M VERY -U users.txt -t target     | enumerate valid users by using a wordlist, this enumerations can be unreliable depending on server configs                                       |
| nmap <IP> -p25 --script smtp-enum-users.nse       | enumerate users by using NSE                                                                                                                     |
| auxiliary/scanner/smtp/smtp_enum                  | use this payload in metasploit to enumerate SMTP                                                                                                 |
| auxiliary/scanner/smtp/smtp_version               | payload in metasploit to get SMTP's version                                                                                                      |
```bash
# Basic SMTP commands
HELO/EHLO    # Identify client to server (EHLO for Extended SMTP)
MAIL FROM    # Specify sender
RCPT TO      # Specify recipient
DATA         # Begin message content
QUIT         # Close connection
VRFY         # Verify user exists
EXPN         # Expand mailing list
AUTH PLAIN   # Authentication (with ESMTP)
RSET         # Reset connection
NOOP         # No operation (prevent timeout)
```


# IMAP/POP3 (143, 993 & 110, 995)
| Command                                                                    | Description                                                                |
| -------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| sudo nmap -sV -sS -sC -p143,993,110,995 \<target>                          | basic scan with default scripts to get some useful information             |
| sudo nmap -p110,143,993,995 --script imap\* pop3\* \<target>               | scan with NSE                                                              |
| nc -nv \<target> <143 or 110>                                              | banner grabbing for unencrypted IMAP-POP3                                  |
| openssl s_client -connect \<target>:<993 or 995>                           | banner grabbing for encrypted IMAP-POP3                                    |
| hydra -l \<user> -P /usr/share/wordlists/rockyou.txt \<pop3/imap>://target | Brute Force Attacks, if you use user list then make option -L userlist.txt |
| curl -k 'imaps://<FQDN/IP>' --user \<user>:\<password>                     | login with curl tool                                                       |
**Some metasploit payloads for enumeration**
use auxiliary/scanner/imap/imap_version
use auxiliary/scanner/imap/imap_login
use auxiliary/scanner/pop3/pop3_version
use auxiliary/scanner/pop3/pop3_login

**IMAP Commands** 

| **Command**                     | **Description**                                                                                               |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `1 LOGIN username password`     | User's login.                                                                                                 |
| `1 LIST "" *`                   | Lists all directories.                                                                                        |
| `1 CREATE "INBOX"`              | Creates a mailbox with a specified name.                                                                      |
| `1 DELETE "INBOX"`              | Deletes a mailbox.                                                                                            |
| `1 RENAME "ToRead" "Important"` | Renames a mailbox.                                                                                            |
| `1 LSUB "" *`                   | Returns a subset of names from the set of names that the User has declared as being `active` or `subscribed`. |
| `1 SELECT INBOX`                | Selects a mailbox so that messages in the mailbox can be accessed.                                            |
| `1 UNSELECT INBOX`              | Exits the selected mailbox.                                                                                   |
| `1 FETCH <ID> all`              | Retrieves data associated with a message in the mailbox.                                                      |
| `1 CLOSE`                       | Removes all messages with the `Deleted` flag set.                                                             |
| `1 LOGOUT`                      | Closes the connection with the IMAP server.                                                                   |

**POP3 Commands**

| **Command**     | **Description**                                             |
| --------------- | ----------------------------------------------------------- |
| `USER username` | Identifies the user.                                        |
| `PASS password` | Authentication of the user using its password.              |
| `STAT`          | Requests the number of saved emails from the server.        |
| `LIST`          | Requests from the server the number and size of all emails. |
| `RETR id`       | Requests the server to deliver the requested email by ID.   |
| `DELE id`       | Requests the server to delete the requested email by ID.    |
| `CAPA`          | Requests the server to display the server capabilities.     |
| `RSET`          | Requests the server to reset the transmitted information.   |
| `QUIT`          | Closes the connection with the POP3 server.                 |


# SNMP (UDP 161,162)

```bash
              SNMP
                │
        ┌───────┴────────┐
        │                │
   Find access       Get information
        │                │
   onesixtyone       snmpwalk / braa 
```

| Command                                                | Description                                            |
| ------------------------------------------------------ | ------------------------------------------------------ |
| nmap -sU -p 161,162 -sV -sC -- script snmp-\*\<target> | Basic UDP scan +SNMP scripts                           |
| snmpwalk -v2c -c \<community string> \<target>         | Querying OIDs using snmpwalk.<br>-v 2c for the SNMPv2c |
| snmpwalk -v3 \<target>                                 | enumerate OIDS for SNMPv3                              |
| onesixtyone -c wordlist.txt \<target>                  | Testing community strings with wordlist                |
| nmap -sU -p 161 --script snmp-brute \<target>          | Testing community strings with NSE                     |
| braa \<community string>@\<IP>:.1.*                    | Bruteforcing SNMP service OIDs                         |

# MySQL (3306)

| Commands                                    | Description                                                               |
| ------------------------------------------- | ------------------------------------------------------------------------- |
| sudo nmap -sV -p3306 \<target>              | service detection by nmap                                                 |
| sudo nmap -sV -sC --script mysql* \<target> | use NSE to get useful info, like valid usernames, Accounts, Version ..etc |
| mysql -u \<user> -p\<password> -h \<target> | Login with creds                                                          |
| mysql -u root -h \<target>                  | testing to login with empty password                                      |

**MySQL Commands** 

| Command                                               | Description                                                |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| mysql -u \<user> -p\<password> -h \<IP address>       | Connect to MySQL server (no space between -p and password) |
| show databases;                                       | Show all databases                                         |
| use \<database>;                                      | Select one of the existing databases                       |
| show tables;                                          | Show all available tables in the selected database         |
| describe \<table>;                                    | Show all names of columns in the table                     |
| show columns from \<table>;                           | Show all columns in the selected table                     |
| select * from \<table>;                               | Show everything in the desired table                       |
| select * from \<table> where \<column> = "\<string>"; | Search for needed string in the desired table              |

# MSSQL (1433)

| Command                                                                                                                                                                                                                                                                                              | Description                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 \<target> | comprehensive Nmap Scan, Look for: version number, server name, named pipes, and any credentials found. |
| use auxiliary/scanner/mssql/mssql_ping                                                                                                                                                                                                                                                               | Scan by metasploit, also look for version and server name ..                                            |
| nmap -p1433 --script ms-sql-info,ms-sql-config,ms-sql-tables \<target>                                                                                                                                                                                                                               | comprehensive simple scan (alternative for big one )                                                    |
| python3 mssqlclient.py user@\<target> -windows-auth                                                                                                                                                                                                                                                  | connect with creds                                                                                      |
| select name from sys.databases                                                                                                                                                                                                                                                                       | list all databases                                                                                      |
```bash
# Get MSSQL version
SQL> SELECT @@version;

# Get server information
SQL> SELECT @@servername;

# Get database information
SQL> SELECT name, database_id FROM sys.databases;

# Get user information
SQL> SELECT name FROM sys.server_principals WHERE type = 'S';

# Get database permissions
SQL> SELECT * FROM sys.database_permissions;
```


# Oracles (1521)

| Command                                                                                                                           | Description                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sudo nmap -p1521 -sV target                                                                                                       | service detection by nmap                                                                                                                                                   |
| sudo nmap -p1521 -sV target --open --script oracle-sid-brute                                                                      | SID Enumeration, by using NSE                                                                                                                                               |
| ./odat.py all -s target                                                                                                           | ODAT Comprehensive Enumeration, to show vulns, valid creds and enumerate all service. all here means use all modules. passwordguesser is a module to know valid credentials |
| sqlplus user@\<target>/\<SID> as sysdba                                                                                           | connect to service with valid creds, as sysdba means try to login as a system admin                                                                                         |
| select table_name from all_tables;                                                                                                | list all databases                                                                                                                                                          |
| select * from user_role_privs;                                                                                                    | Check elevated privileges                                                                                                                                                   |
| select name, password from sys.user$;                                                                                             | Extract password hashes from sys.user$                                                                                                                                      |
| ./odat.py utlfile -s \<target> -d \<XE> -U \<user> -P \<pass> --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./<testing.txt> | check if you can put a file in the web server <br>linux : /var/www/html<br>windows : C:\inetpub\wwwroot                                                                     |
| curl -X GET http://target/testing.txt                                                                                             | Verify File Upload                                                                                                                                                          |


# IPMI (UDP 623)

| Command                                                                   | Description                                                                                                           |
| ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| sudo nmap -sU --script ipmi-version -p 623 \<target>                      | use NSE to get version of the service                                                                                 |
| nmap -sU -p623 --script ipmi-cipher-zero target                           | scan by a nmap script, used to detect an authentication bypass vulnerability in (IPMI) v2.0 devices                   |
| use auxiliary/scanner/ipmi/ipmi_version                                   | payload in metasploit to get the version                                                                              |
| use auxiliary/scanner/ipmi/ipmi_dumphashes                                | payload in meta, used to IPMI dumphashes (for cipher zero vulnerability). Get hash_passwords if the target vulnerable |
| use auxiliary/scanner/ipmi/ipmi_cipher_zero                               | Scan the target to find servers running IPMI that are vulnerable to an authentication bypass (cipher-zero)            |
| hashcat -m 7300 --username ipmi_hashes.txt wordlist.txt                   | Crack hashes with hashcat, --username is for skip prefix in the hash format of ipmi                                   |

# SSH(22)

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

# R-Services(513,514)

| Command                              | Description                                                                                                    |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| nmap -sV -p513,514 target            | Check for R-Services                                                                                           |
| nc target \<513 or 514>              | Banner grabbing                                                                                                |
| rsh target -l \<username> \<command> | try to perform a command on the target, you can try without a username if target trust all user or hosts--> ++ |
| rlogin target -l \<username>         | login If .rhostshas + + — you are in immediately with no password.                                             |



# RDP(3389)

| Command                                                       | Description                                                                |
| ------------------------------------------------------------- | -------------------------------------------------------------------------- |
| nmap -p3389 -sV -sC target                                    | Nmap RDP detection                                                         |
| nmap -p3389 --script rdp-enum-encryption,rdp-ntlm-info target | RDP security enumeration                                                   |
| nmap -p3389 --script rdp-vuln* target                         | RDP vulnerability scanning                                                 |
| nmap -p3389 --script ssl-cert target                          | Certificate analysis                                                       |
| ./rdp-sec-check.pl target                                     | identify the security settings of RDP servers based on the handshakes.     |
| xfreerdp /u:\<user> /p:\<password> /v:target                  | Authentication testing and try to login with different users and passwords |
| rdesktop -u administrator -p password target                  | Authentication testing                                                     |


# WinRM(5985,5986)

| Commands                                   | Description                  |
| ------------------------------------------ | ---------------------------- |
| nmap -p5985,5986 -sV -sC target            | Nmap WinRM detection         |
| nmap -p5985 --script http-auth target      | WinRM authentication testing |
| evil-winrm -i target -u \<user> -p \<pass> | Interact with WinRM          |