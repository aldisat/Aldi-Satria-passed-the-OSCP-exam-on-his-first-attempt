# 0. Enum
## a. `Sudo -l`
## b. `netstat -tunlp` / `ss -tulwn`
cek listerning local port
![](Attachments/Pasted%20image%2020260925103607.png)
cek siapa dan apa yang menjalankan
![](Attachments/Pasted%20image%2020260925103707.png)
kalau yang menjalankan root, jika bisa kita exploitasi akan dapat root
coba buka port tersebut dengan port forwarding
```shell
ssh -L 9229:localhost:9229 engineer@reactor.htb
```
![](Attachments/Pasted%20image%2020260925104016.png)