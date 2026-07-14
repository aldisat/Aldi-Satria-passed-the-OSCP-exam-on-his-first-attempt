**tools untuk memetakan jalur serangan di Active Directory** — visualisasi siapa punya akses ke apa, dan gimana caranya sampai ke Domain Admin.
Kapan mulai pake bloodhound?
==setelah punya credential atau akses ke domain, sekecil apapun itu==

dahulukan credential plaintext untuk generate bloodhound

# Mapping Serangan
## Colect data
dahulukan credential plaintext untuk generate bloodhound
```shell
bloodhound-python -u 'wallace.everette' -p 'Welcome2026@' -dc 'DC01.logging.htb' -d 'logging.htb'  --dns-tcp -ns 10.129.198.221 --dns-timeout 10 --zip -c All
```

kalau tidak bisa, gunakan nxc
```shell
nxc ldap 10.129.5.193 -u 'alex.turner' -p 'Checkpoint2024!' --bloodhound -c All 
```

kalau tidak bisa juga, gunakan bloodyad
```shell
bloodyad --host dc01.checkpoint.htb -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get bloodhound
```
![](Attachments/Pasted%20image%2020260622145414.png)
## Mark "OWNED"
semua akun yang dimiliki, termasuk:
- user password
- TGT
- shell

## Cek Outbound Object Control
apa yang bisa Akun ini kontrol.

| ACL           | Attack                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------ |
| Write Owner   | can change the owner of a group or modify the access control list (ACL) Management for the group |
| Generic All   | Full Control                                                                                     |
| Generic Write | Shadow Credential Attack                                                                         |

# Error
## deadbeef
problem -> ipv6 active
![](Attachments/Pasted%20image%2020260622134301.png)
solusi -> disable
```shell
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
ip a | grep inet6
```