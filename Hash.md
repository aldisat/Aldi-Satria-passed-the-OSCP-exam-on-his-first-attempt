# Identify
## Online
- [Hash Type Identifier - Identify unknown hashes](https://hashes.com/en/tools/hash_identifier) 
![](Pasted%20image%2020260616130343.png)
- [example_hashes [hashcat wiki]](https://hashcat.net/wiki/doku.php?id=example_hashes) 

## Tools
```shell
hash-identifier -h
hashid -m
hashcat --identify
```


![](Pasted%20image%2020260616094624.png)

| Angka | Nama                 | Algoritma | Hashcat Mode |
| ----- | -------------------- | --------- | ------------ |
| 17    | RC4-HMAC (old)       | RC4       | 13100        |
| 23    | RC4-HMAC             | RC4/MD5   | **13100**    |
| 18    | AES256-CTS-HMAC-SHA1 | AES-256   | **19700**    |
| 17    | AES128-CTS-HMAC-SHA1 | AES-128   | 19600        |
# Cracks
gunakan `--force` untk virtual machine
```shell
hashcat -m 13100 kerberoasting_a.white_adm.txt /usr/share/wordlists/rockyou.txt --force

```
## Cracks successed
## Cracks Failed
![](Pasted%20image%2020260616133245.png)