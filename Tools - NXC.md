# Test Credentials
```shell
for svc in smb winrm rdp ssh ldap mssql ftp; do
  echo "===$svc==="
  nxc $svc 10.129.27.241 -u 'anderson.w' -p 'R3dT3am@Acc3ss#01'
done 
```
# Account Restriction
Artinya akun valid tapi di restrict oleh server
![](Attachments/Pasted%20image%2020260703141136.png)

# Signing None pada LDAP
bisa terjangging NTLM relay attack pada LDAP
![](Attachments/Pasted%20image%2020260716105957.png)

# Cek admin
kalau PWN! berarti akun admin
```shell
# cek local admin 
nxc smb garfield.htb -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' --local-auth 

# cek domain context
nxc smb garfield.htb -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978'
```
![](Attachments/Pasted%20image%2020260716171541.png)