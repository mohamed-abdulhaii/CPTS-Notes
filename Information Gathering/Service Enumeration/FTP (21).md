# Cheat Sheet

| command                                                            | Description                                                                                                                                                |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sudo nmap -sV -p21 -sC -A \<target\>                               | Run an aggressive scan , try to get the version , use the default scripts                                                                                  |
| sudo nmap -p21 --script \<name of script\>                         | use a specific NSE to run on the target, you can add ==--script-trace== to  provides the ability to trace the progress of NSE scripts at the network level |
| nc -nv <FQDN/IP> 21                                                | Interact with the FTP service , and grab the banner                                                                                                        |
| telnet <FQDN/IP> 21                                                | Interact with the FTP service , and grab the banner                                                                                                        |
| openssl s_client -connect <FQDN/IP>:21 -starttls ftp               | Interact with the FTP service on the target using encrypted connection.                                                                                    |
| use auxiliary/scanner/ftp/(ftp_version or anonymous) in metasploit | use modules in metasploit , gives more info                                                                                                                |
Steps 
- [ ] Scan target ports : Run default scripts & version detection
- [ ] Anonymous Login Check 
- [ ] Manual Interaction & Banner Grabbing : Connect with Netcat or Telnet, Capture the raw service banner to spot hidden details or outdated software.
- [ ] if you login, check for sensitive files by ls -lat
# Related Articles  

[[FTP Commands]] : when you get inside and try download or read files on the target  

# Overview 

**The File Transfer Protocol FTP** is one of the oldest protocols on the Internet. The FTP runs within the application layer of the TCP/IP protocol stack , used to upload or download files to server , FTP is a clear-text , Supports Authentication

**FTP** works with two ports one (21) for connection and commands and one (20) for download files or uploads (data sharing)

***FTP*** have two modes active and passive.
**In Active mode**, the server starts the data connection after the client start the connection at port 21 ( the client’s firewall usually block it because it’s a connect comes from outside)
**In Passive mode**, the client start both connection 

there are a Dangerous Settings as a pentester , you should care about these settings

|`anonymous_enable=YES`    :             Allowing anonymous login? 
|`anon_upload_enable=YES`|             Allowing anonymous to upload files?|
|`anon_mkdir_write_enable=YES`|    Allowing anonymous to create new directories?|
|`no_anon_password=YES`|                 Do not ask anonymous for password?|
|`anon_root=/home/username/ftp`|  Directory for anonymous.|
|`write_enable=YES`|                         Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO,                                                             MKD, RMD, APPE, and SITE?

- here with these settings you can make a lot of things like Allows unauthorized users to access or modify files.