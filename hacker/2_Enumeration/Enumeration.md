**Enumeration Techniques for Ethical Hacking**
=> This document outlines practical enumeration techniques for network reconnaissance in ethical hacking, focusing on real-world application with minimal theory unless necessary for clarity.

**1. What is Enumeration?**
Enumeration is the active process of probing a target system to extract detailed information about its resources, configurations, and vulnerabilities. It is typically performed in the reconnaissance phase of penetration testing.

Key Activities:
- Active connections: Query services (e.g., SMB, LDAP, NetBIOS) to gather data.
- Directed queries: Target specific protocols/services for details like users or shares.
- Identify attack points: Discover misconfigurations (e.g., open shares, weak credentials).
- Intranet environment: Often LAN-focused due to protocol restrictions (e.g., NetBIOS, SMB).
Note: Enumeration gathers data for later exploitation (e.g., password attacks), but does not include the attacks themselves.

**2. Information Enumerated**
Enumeration targets the following data to map networks and identify vulnerabilities:
- Network resources: IP ranges, live hosts, open ports, services (e.g., SMB, LDAP, SNMP).
- Network shares: SMB/NFS shared folders/drives, potentially exposing sensitive data.
- Routing tables: Network paths/devices for topology mapping.
- Audit & service settings: Service versions, misconfigurations (e.g., SNMP community strings).
- SNMP & FQDN details: SNMP for device configs; FQDNs for host/domain identification.
- Machine names: Hostnames or NetBIOS names (e.g., “DC01” for domain controllers).
- Users and groups: Valid usernames, group memberships, admin accounts.
- Applications and banners: Software versions (via banner grabbing) for vulnerability lookup.
- Policies: Password policies or account lockout settings to inform brute-force strategies.

**3. NetBIOS Enumeration (LAN Only)**
NetBIOS enumeration extracts machine names, shares, and users over NetBIOS (ports 137-139). It’s LAN-only due to broadcast traffic limitations.
Tools and Commands:
```
Nmap (Kali):nmap --script=nbstat -Pn <IP>
```
-Pn: Skips host discovery.
Outputs: NetBIOS name, workgroup, MAC address.

```
nbtstat (Windows):nbtstat -a <IP>
```
Shows NetBIOS name table (machine name, domain, services like <20> for file sharing).

```
nbtscan:nbtscan <IP_range>
```
Scans multiple IPs for NetBIOS info.

Practical Use:
- Reveals system roles (e.g., “FILESERVER01”) or misconfigurations (e.g., open shares).
- Security Note: NetBIOS is often disabled in modern networks (post-Windows XP). Verify with:nmap -p 137-139 <IP>

4. Net View (Windows, LAN Only)
Lists shared folders on a target system, including hidden shares (e.g., ADMIN$, C$), if accessible.
Command:
```
net view \\<IP> /ALL
```
Requires SMB connectivity (ports 445/139) and credentials or null sessions.
```
Alternative:net view /DOMAIN:<domain_name>
```
Lists all domain hosts for network mapping.

Practical Tip:
- Accessible shares without credentials (e.g., “Everyone” permissions) indicate critical misconfigurations.
- Security Note: SMBv1 is often disabled due to vulnerabilities (e.g., EternalBlue). Check SMB version:nmap --script=smb-protocols -p445 <IP>

**5. LDAP Enumeration (LAN Only)**
LDAP (Lightweight Directory Access Protocol) queries directory services (e.g., Active Directory) for users, groups, computers, and organizational units (OUs). Runs on TCP 389 (or 636 for LDAPS).
Key Concepts:
- Process: Clients connect to a Directory System Agent (DSA, typically a Domain Controller) to query usernames, emails, or department details.
- Domain Controller (DC): Manages authentication/authorization in Active Directory.
- Active Directory (AD): Microsoft’s directory service for network resources.
- SMB Integration: SMB supports file/printer sharing and can enumerate shares or authenticate to AD.

Commands:
```
List SMB shares:smbclient -L \\<IP> -U administrator
```
Requires credentials or null session (rare in modern systems).

```
Connect to a share:smbclient \\<IP>\<share_name> -U administrator
```
```
List shares and permissions:smbmap -H <IP> -u administrator
```
Shows share names, permissions (e.g., READ, WRITE), and sometimes sensitive files.

```
ldapsearch (Kali):ldapsearch -x -h <IP> -b "dc=domain,dc=com"
```
Queries LDAP for users, groups, or OUs.

Practical Tip:
- Requires credentials or anonymous access (rare in secure setups). Test weak credentials.
- Security Note: LDAPS (encrypted) is common. Use -p 636 for LDAPS if 389 fails.

Clarification:
SMB enumerates shares or authenticates, but AD attacks typically exploit Kerberos (e.g., Kerberoasting) or NTLM (e.g., pass-the-hash), not SMB directly.

6. Enum4linux (Kali, LAN Only)
Automates enumeration of SMB, NetBIOS, and LDAP data, extracting users, groups, shares, and policies.
Command:
```
enum4linux -u administrator -p <password> -U <IP>
```
-U: Enumerates users.
-a: Runs all tasks (users, shares, groups, policies).
-S: Lists SMB shares.

Practical Tip:
- Noisy tool (generates traffic). Use cautiously in stealthy engagements.
- Output includes NetBIOS names, RID cycling (user IDs), shares, and password policies.

7. Additional Enumeration Techniques
SNMP Enumeration:
```
Query SNMP (port 161/UDP) for device configs:onesixtyone -c community.txt <IP>
```
Uses common community strings (e.g., “public”).

RPC Enumeration:
```
Query RPC services (port 135):rpcclient -U "" <IP>
```
Extracts users, groups, or domain info if null sessions are enabled.

DNS Enumeration:
```
Gather subdomains, MX records, or zone transfers:dnsenum <domain>
```

Kerberos Enumeration:
```
Enumerate valid AD usernames without triggering lockouts:kerbrute userenum -d <domain> <userlist.txt> <DC_IP>
```
Useful for password spraying.

8. General Notes
- Stealth: Active enumeration (e.g., enum4linux, smbclient) generates noticeable traffic. Use passive methods or low-rate scans for stealth.
- Modern Networks: NetBIOS and SMBv1 are often disabled. Verify services:nmap -p 137-139,445 <IP>
- Credentials: Modern systems require valid credentials. Test default/weak credentials (e.g., administrator:password).
- Privilege Escalation: Enumeration feeds into exploitation (e.g., misconfigured shares, weak accounts).


Last updated: June 26, 2025
