# Cheat Sheet 

| Command                                                                                                                                                                                                                                                                                              | Description                                                                                             |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 \<target> | comprehensive Nmap Scan, Look for: version number, server name, named pipes, and any credentials found. |
| use auxiliary/scanner/mssql/mssql_ping                                                                                                                                                                                                                                                               | Scan by metasploit, also look for version and server name ..                                            |
| nmap -p1433 --script ms-sql-info,ms-sql-config,ms-sql-tables \<target>                                                                                                                                                                                                                               | comprehensive simple scan (alternative for big one )                                                    |
| python3 mssqlclient.py user@\<target> -windows-auth                                                                                                                                                                                                                                                  | connect with creds                                                                                      |
| select name from sys.databases                                                                                                                                                                                                                                                                       | list all databases                                                                                      |
- [ ] Port scan for 1433
- [ ] Service version detection
- [ ] Hostname extraction
- [ ] Authentication method testing
- [ ] Default credential testing (test sa, admin, administrator,....)
- [ ] Database enumeration
- [ ] System database analysis
- [ ] Custom database discovery
- [ ] User and permission assessment (users for other services maybe you found it in database)

# Related Articles

MSSQL Enumeration : [link](obsidian://open?vault=pentest%20(CPTS)&file=Information%20Gathering%2FService%20Enumeration%2FMSSQL(1433%20TCP)%2FMSSQL%20Enumeration), once you inside 
MSSQL Attack Vector : [link](obsidian://open?vault=pentest%20(CPTS)&file=Information%20Gathering%2FService%20Enumeration%2FMSSQL(1433%20TCP)%2FMSSQL%20Attack%20Vector), new subject you will study
# Overview

**Microsoft SQL (MSSQL)** is Microsoft's SQL-based relational database management system. Unlike MySQL, which is open-source, MSSQL is closed source and was initially written to run on Windows operating systems. It is popular among database administrators and developers when building applications that run on Microsoft's .NET framework due to its strong native support for .NET.

**Key Characteristics:**
- **Port 1433**: Default MSSQL port
- **Authentication**: Windows Authentication or SQL Server Authentication
- **Default Instance**: MSSQLSERVER
- **Protocol**: Tabular Data Stream (TDS)
- **Platform**: Primarily Windows (Linux/MacOS versions available)

An **MSSQL client** is a software application or tool used to connect, send queries, and manage a Microsoft SQL Server (MSSQL) database
common used : SQL Server Management Studio (SSMS)

### Default System Databases
MSSQL has default system databases that help understand the structure of all databases hosted on a target server:

| Database     | Description                                                               |
| ------------ | ------------------------------------------------------------------------- |
| **master**   | Tracks all system information for an SQL server instance                  |
| **model**    | Template database that acts as a structure for every new database created |
| **msdb**     | Used by SQL Server Agent to schedule jobs & alerts                        |
| **tempdb**   | Stores temporary objects                                                  |
| **resource** | Read-only database containing system objects included with SQL server     |

### Default Configuration

**Initial Setup**:
When an admin initially installs and configures MSSQL to be network accessible:
- **Service Account**: SQL service runs as `NT SERVICE\MSSQLSERVER` (virtual account to use)
- **Authentication**: Windows Authentication by default
- **Encryption**: Not enforced by default
- **Access Control**: Uses Windows OS for authentication processing

### Authentication Methods

1. **Windows Authentication**:
    - Uses local SAM database or domain controller
    - Integrates with Active Directory
    - Can lead to privilege escalation if compromised
2. **SQL Server Authentication**:
    - Uses database-specific user accounts
    -  Independent of Windows authentication

### Dangerous Settings

Common misconfigurations that can lead to security issues:

|**Setting**|**Risk Level**|**Description**|
|---|---|---|
|**No encryption**|High|MSSQL clients not using encryption to connect|
|**Self-signed certificates**|Medium|Can be spoofed during attacks|
|**Named pipes enabled**|Medium|Additional attack surface|
|**Default SA credentials**|Critical|Weak or unchanged SA account passwords|
|**SA account enabled**|High|Admins may forget to disable default SA account|
