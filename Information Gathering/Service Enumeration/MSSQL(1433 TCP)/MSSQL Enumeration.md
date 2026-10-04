
## Database Information Gathering

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


## System Information

```bash
# Get system configuration
SQL> SELECT name, value FROM sys.configurations WHERE name = 'xp_cmdshell';

# Get linked servers
SQL> SELECT * FROM sys.servers;

# Get database files
SQL> SELECT name, physical_name FROM sys.master_files;
```