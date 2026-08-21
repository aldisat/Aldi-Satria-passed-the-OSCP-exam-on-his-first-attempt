# TCP
```shell
sudo nmap -sS 10.129.27.241 --min-rate=1000 -p- -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.27.241 -Pn -o alltcpport.txt
```

# UDP
```shell
sudo nmap -sU 10.129.27.241 --min-rate=1000 -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.27.241 -Pn -o alludpport.txt
```