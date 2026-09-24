# 1. Account
==l.wilson_adm== -> ini artinya admin
==Email_svc== -> kalau ada ini akun emailsvc berarti ada email service di windows AD
# 2. Domain Controller
Domain controller -> DC01.garfield.htb
Domain -> garfield.htb
# 3. AES256
Standar enkripsi kerberos
# 4. SID
Security Identifier, nomor unik untuk mengidentifikasi user, group dan komputer
```shell
rpcclient -U 'danglingtree.htb/jake.h%NewPassword123' 10.129.39.55

# Domain Sid
rpcclient $> lsaquery

# Specific Account
rpcclient $> lookupnames administrator
```
![](Attachments/Pasted%20image%2020260818144854.png)
# 5. RODC
RODC -> Read Only Domain Controller
tanda kalau ada RODC environtment adalah ada akun krbtgt_8245
# 6. DPAPI
Data Protection API
# 7. ADCS
## a. Certificate Authority (CA)
Server yang menerbitkan & menandatangani sertifikat (biasanya `<domain>-CA`)
![](Attachments/Pasted%20image%2020260910131445.png)
## b. Certificate Template
Blueprint sertifikat, siapa yang boleh request cert, EKUnya apa
![](Attachments/Pasted%20image%2020260910131816.png)
## c. Extended Key Usage (EKU)
Menentukan tujuan sertifikat: `Client Authentication` (OID `1.3.6.1.5.5.7.3.2`) adalah kunci abuse untuk login
## d. PKI
Public Key Infractructure, its implemented using Active Directory Certificate Service (ADCS)
# 8. DN (Dis)
# 9. OID
Object Identifier
# 10. LDIF
LDAP Data Intechange Format
# 11. DACL
An Access Control List (ACL is a general term for a list of rules that define permissions on an object or network traffic, while a (DACL) is a specific type of ACL used in operating systems like Windows to control whether users and groups can access a specific resource.