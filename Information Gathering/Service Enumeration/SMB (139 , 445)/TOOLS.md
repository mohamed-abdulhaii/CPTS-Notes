## rpc_client 

rpc : (Remote Procedure Call ) is a way for a program to run a function on another computer in a network as if it were local.
(deal with the smb service to get info , by perform function or code into the service )

command to start a session 
- rpcclient -U ""  \<IP\>


The  rpcclient offers us many different requests with which we can execute specific functions on the SMB server to get information, for more use man page of tool

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


## smbmap
its a tool to deal with smb (do actual service like download or upload files)

| command                                      | Description                                      |
| -------------------------------------------- | ------------------------------------------------ |
| smbmap -H target                             | Enumerating SMB shares                           |
| smbmap -H target -u user -p pass             | if you have creds                                |
| smbmap -H target -r share                    | Recursive network share enumeration using smbmap |
| smbmap -H target --download "share\file.txt" | Download a specific file from the shared folder  |
