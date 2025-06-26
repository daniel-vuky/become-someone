**1. What is it?**
- Creating active connections with a target system
- Performing directed queries
- Identify points for a system attack
- Perform password attacks
- Intranet environment

**2. Information enumerated by us:**
- Network resources
- Network shares
- Routing Tables
- Audit & service settings
- [SNMP](https://www.fortra.com/resources/articles/snmp-basics-what-it-and-how-it-works) & [FQDN](https://www.f5.com/glossary/fqdn) details
- Machine names
- Users and groups
- Applications and banners

**3. NetBIOS Enumeration - LAN only**
=> Scan device name
* Nmap (Kali)
  Using nmap script
  ```
  nmap --script=nbstat -Pn <IP>
  ```
* nbtstat (WINDOW)
  ```
  nbtstat -a <IP>
  ```
  
**4 Net view (WINDOW) - LAN only**
=> Scan all public shared folders (everyone access)
```
net view \\<IP> /ALL
```

**5. LDAP Enumeration**
- This is an Internet Protocol for accessing a distributed directory service
- A client starts an LDAP session by connecting to a directory system agent (DSA) on TCP port 389 and then sends an operation request to the DSA
=> We query the LDAP service to gather information, such as valid **username, address, department details...**

- Domain Controller (DC): A central server that manages authentication and authorization in an Active Directory environment.
- Active Directory (AD): A directory service system that manages users, computers, groups, and other resources in a Windows network.
- SMB Server: Provides file, folder, and printer sharing services via the SMB protocol, often deeply integrated with AD for access control.
=> Request to AD/DC using the SMB protocol => attack AD/DC by the SMB protocol

* If we had an administrator account =>
We could scan the shared folder from the SMB server
```
smbclient -L \\<IP> -U administrator
```
Connect to the folder
```
smbclient \\<IP> -U administrator
```
Other way to list share folder and permission from SMB
```
smbmap -H <IP> -u administrator
```











