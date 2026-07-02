# TCP
```shell
nmap -sS 10.129.244.72 --min-rate=1000 -p- -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.244.72 -Pn -o alltcpport.txt
```

# UDP
```shell
nmap -sU 10.129.244.72 --min-rate=1000 -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sCV -p {} 10.129.244.72 -Pn -o alludpport.txt
```