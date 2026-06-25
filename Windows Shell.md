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
whoami /priv
whoami /group
```
![](Attachments/Pasted%20image%2020260625053212.png)
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
net user
net localgroup administrator
```
![](Attachments/Pasted%20image%2020260625054311.png)
# Schedule Task
```powershell
# Versi CMD  
schtasks /query /tn "UpdateChecker Agent" /fo list /v  
  
# Versi Powershell  
Get-ScheduledTask -TaskName "UpdateChecker Agent"  
  
# Versi Manual  
PS C:\Share> $s = New-Object -ComObject "Schedule.Service"; $s.Connect(); $s.GetFolder("\").GetTask("UpdateChecker Agent").Definition.Principal
```

# Download
```powershell
|download "C:\Program Files\UpdateMonitor\UpdateMonitor.exe"||
```




