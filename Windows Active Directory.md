```plain
urutan enum
1. cek credential
2. smb
3. ldap
4. bloodhound
5. bloodyad
6. http
```

# Account
l.wilson_adm -> ini artinya admin
# Domain Controller
Domain controller -> DC01.garfield.htb
Domain -> garfield.htb
# AES256
Standar enkripsi kerberos
# SID
Security Identifier, nomor unik untuk mengidentifikasi user, group dan komputer
```shell
rpcclient -U 'garfield.htb/j.arbuckle%Th1sD4mnC4t!@1978' 10.129.244.207

# Domain Sid
rpcclient $> lsaquery

# Specific Account
rpcclient $> lookupnames administrator

```
![](Attachments/Pasted%20image%2020260818144854.png)
# Email_svc
kalau ada ini akun emailsvc berarti ada email service di windows AD
# RODC
RODC -> Read Only Domain Controller
tanda kalau ada RODC environtment adalah ada akun krbtgt_8245