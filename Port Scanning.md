# TCP
```shell
nmap -sS 10.10.11.69 --min-rate=1000 -p- -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sV -p {} 10.10.11.69 -Pn -o alltcpport.txt
```

# UDP
```shell
nmap -sUC 10.10.11.69 --min-rate=1000 --top-ports 1000  -Pn -open | grep "\/tcp" | sed 's/\/tcp.*//' | tr "\n" , | sed 's/.$//' | xargs -I {} nmap -sV -p {} 10.10.11.69 -Pn -o alludpport.txt
```