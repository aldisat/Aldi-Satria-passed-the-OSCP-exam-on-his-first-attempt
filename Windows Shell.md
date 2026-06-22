# User.txt
## Cari User di directory lain
```powershell
# Cari di semua Desktop semua user
dir C:\Users\*\Desktop\user.txt /s /b 2>nul

# Atau lebih luas
dir C:\Users\ /s /b 2>nul | findstr /i "user.txt\|local.txt\|proof.txt"
```


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




