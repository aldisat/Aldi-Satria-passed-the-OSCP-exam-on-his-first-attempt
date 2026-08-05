# TCP
```shell
sudo nmap -sS garfield.htb --min-rate=1000 -p- -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} garfield.htb -Pn -o alltcpport.txt
```

# UDP
```shell
sudo nmap -sU 10.129.244.207 --min-rate=1000 -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.244.207 -Pn -o alludpport.txt
```