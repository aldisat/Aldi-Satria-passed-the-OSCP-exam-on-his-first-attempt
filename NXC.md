# Test Credentials
```shell
for svc in smb winrm rdp ssh ldap mssql ftp; do
  echo "===$svc==="
  nxc $svc 10.129.1.107 -u 'wallace.everette' -p 'Welcome2026@'
done
```
# Account Restriction
Artinya akun valid tapi di restrict oleh server
![](Attachments/Pasted%20image%2020260703141136.png)

