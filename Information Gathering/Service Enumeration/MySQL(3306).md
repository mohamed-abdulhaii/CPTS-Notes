# Cheat Sheet 

| Commands                                    | Description                                                               |
| ------------------------------------------- | ------------------------------------------------------------------------- |
| sudo nmap -sV -p3306 \<target>              | service detection by nmap                                                 |
| sudo nmap -sV -sC --script mysql* \<target> | use NSE to get useful info, like valid usernames, Accounts, Version ..etc |
| mysql -u \<user> -p\<password> -h \<target> | Login with creds                                                          |
| mysql -u root -h \<target>                  | testing to login with empty password                                      |
- [ ] Port scan for 3306
- [ ] Service version detection
- [ ] Default credential testing
- [ ] Anonymous access testing
- [ ] Database enumeration
- [ ] User account discovery
- [ ] Privilege assessment
- [ ] Configuration analysis
- [ ] Data extraction testing

# Related Articles

SQL Commands : [Link](obsidian://open?vault=pentest%20(CPTS)&file=SQL%20Commands), once you inside 
# Overview 

**MySQL** is a database System (open-source system). A database is like a big organized file that stores all the data for a website — usernames, passwords, emails, posts. As a pentester, you care about MySQL because if you get inside it, you can read all the passwords and user data of the entire application.

MariaDB, a popular fork of MySQL, it's also open-source. It use MySQL commands to interaction

**MySQL** stores everything for web apps. WordPress, Joomla, any PHP website — all their users and passwords live in MySQL. If you find a WordPress site, the database is your target.

**Key Characteristics:**

- **Port 3306**: Default MySQL port
- **Protocol**: MySQL native protocol over TCP
- **Authentication**: Username/password based
- **Default Users**: root, mysql
- **File Extension**: .sql files (e.g., wordpress.sql)

Three dangerous misconfigurations to remember:

| Misconfiguration   | Description                                                               | Risk     |
| ------------------ | ------------------------------------------------------------------------- | -------- |
| `user`             | Sets which user the MySQL service will run as                             | High     |
| `password`         | Sets the password for the MySQL user                                      | Critical |
| `admin_address`    | IP address for TCP/IP connections on administrative network interface     | High     |
| `sql_warnings`     | Controls whether single-row INSERT statements produce information strings | Medium   |
| `secure_file_priv` | Limits the effect of data import and export operations                    | High     |
| `debug`            | Indicates current debugging settings                                      | Medium   |
**Security Note**: Sensitive data like passwords can be stored in plain-text form by MySQL, but are generally encrypted by PHP scripts using secure methods like One-Way-Encryption.