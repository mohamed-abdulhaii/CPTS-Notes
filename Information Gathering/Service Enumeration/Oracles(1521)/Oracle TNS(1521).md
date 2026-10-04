
# Cheat Sheet 

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
- [ ] Port scan for 1521
- [ ] Service version detection
- [ ] SID enumeration
- [ ] Credential testing
- [ ] Database connection
- [ ] Privilege escalation testing (try to login with as sysadb)
- [ ] Password hash extraction
- [ ] File upload capabilities
- [ ] Web shell deployment

# Related Articles
Odat tool modules : [link](obsidian://open?vault=pentest%20(CPTS)&file=Articles%2FOdat%20modules)
Attack Path : [link](obsidian://open?vault=pentest%20(CPTS)&file=Information%20Gathering%2FService%20Enumeration%2FOracles(1521)%2FAttack%20Vectors)
# Overview

**The Oracle Transparent Network Substrate (TNS)** server is a communication protocol that facilitates communication between Oracle databases and applications over networks. Initially introduced as part of the Oracle Net Services software suite, TNS supports various networking protocols between Oracle databases and client applications, such as IPX/SPX and TCP/IP protocol stacks.

**Key Characteristics:**

- **Port 1521**: Default Oracle TNS port
- **Authentication**: Username/password
- **SID**: System Identifier for database instances (Think of it like the database's unique name. You MUST know the SID before you can connect)
- **Protocol**: Oracle Native Network Protocol
- **Industries**: Healthcare, finance, retail (large, complex databases)

**Security Features** :

- **Host Authorization**: Accepts connections only from authorized hosts
- **Basic Authentication**: Uses hostnames, IP addresses, usernames, and passwords
- **Encryption**: Oracle Net Services encrypts client-server communication


**TNS have Two Configuration Files** 
1- tnsnames.ora (Client-side) : Define the Host(database server), port, service name, ...
2-listener.ora (Server-side) : Define listener process properties(controls what the listener accepts).

### Oracle Different versions

**Default user & password for Different versions** : 

| Username | Password            | Service                |
| -------- | ------------------- | ---------------------- |
| `scott`  | `tiger`             | Classic Oracle default |
| `dbsnmp` | `dbsnmp`            | Oracle DBSNMP service  |
| `sys`    | `CHANGE_ON_INSTALL` | Oracle 9 default       |

**Oracle TNS is often used with:**

- Oracle DBSNMP
- Oracle Application Server
- Oracle Enterprise Manager
- Oracle Fusion Middleware
- Web servers
- Legacy services (like finger service)

Oracle databases can be protected using **PL/SQL Exclusion List**, a built-in security feature used in Oracle web application environments. Its primary purpose is to **block direct browser access** to sensitive, low-level database packages and schemas to prevent attackers from executing unauthorized backend code

- **Location**: `$ORACLE_HOME/sqldeveloper` directory
- **Purpose**: Text file containing PL/SQL packages to exclude from execution
- **Function**: Serves as a blacklist for Oracle Application Server
- **Implementation**: Loaded into database instance for package restrictions


Tools
- ODAT : it is an open-source penetration testing tool written in Python and designed to enumerate and exploit vulnerabilities in Oracle databases. It can be used to identify and exploit various security flaws in Oracle databases, including SQL injection, remote code execution, and privilege escalation.

- sqlplus : the official Oracle client. You use it to connect and run commands