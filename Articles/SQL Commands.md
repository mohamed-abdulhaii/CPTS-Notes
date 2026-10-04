### Basic Commands

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


### Advanced Query Examples

```bash
# Database exploration
SHOW DATABASES;
USE customers;
SHOW TABLES;
DESCRIBE customers;

# Data extraction
SELECT * FROM customers;
SELECT * FROM customers WHERE name = 'Otto Lang';
SELECT email FROM customers WHERE name = 'Otto Lang';

# User enumeration
SELECT User, Host FROM mysql.user;
SELECT * FROM mysql.user WHERE User='root';
```