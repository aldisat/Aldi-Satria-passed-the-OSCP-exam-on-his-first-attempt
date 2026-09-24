# Test Credentials
```shell
for svc in smb winrm rdp ssh ldap mssql ftp; do
  echo "===$svc==="
  nxc $svc scaffold.htb -u 'j.harris' -p 'Harr1sHelpdesk2026!Breach'
done 

for svc in smb winrm rdp ssh ldap mssql ftp; do
  echo "===$svc==="
  nxc $svc scaffold.htb -u 'm.carter' -p 'BlueSkyTempReset2026!'
done 
```
# Failed
## a. Account Restriction
Artinya akun valid tapi di restrict oleh server
![](Attachments/Pasted%20image%2020260703141136.png)
## b. Account Disabled
# Cek admin
kalau PWN! berarti akun admin
```shell
# cek local admin 
nxc smb garfield.htb -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' --local-auth 

# cek domain context
nxc smb garfield.htb -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978'
```
![](Attachments/Pasted%20image%2020260716171541.png)

# [2. Enumeration](Terminal/Powershell.md#2.%20Enumeration)
