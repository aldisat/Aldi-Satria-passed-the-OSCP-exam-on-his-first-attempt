# TCP
```shell
# Coba yang sederhana dulu
sudo nmap -sCV 10.129.43.5 -Pn -o alltcpport.txt

# Baru ini
sudo nmap -sCV 10.129.246.248 -Pn -p- -o alltcpport_pall.txt

# Otomatis
sudo nmap -sS 10.129.246.248 --min-rate=1000 -p- -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.27.241 -Pn -o alltcpport.txt
```

# UDP
```shell
sudo nmap -sU 10.129.246.248 --min-rate=1000 -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.27.241 -Pn -o alludpport.txt
```

# Rustscan
```shell
# TCP
rustscan -a 10.129.246.248 -- -sC -sV -o alltcpports.txt

# UDP
rustscan --udp -a 10.129.246.248 -- -sC -sV -o alludpports.txt
```
![](Attachments/Pasted%20image%2020260918152623.png)
# Evading IPS/IDS
1. [Source Ports](Network/Firewall.md#Source%20Ports)
