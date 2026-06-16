# Null Session
```shell
smbclient -L //10.129.244.81/ -N
smbclient //10.129.244.81/<share> -N

nxc smb 10.129.244.81 -u '' -p ''

smbmap -H 10.129.244.81 -u Guest -p ''
```

# Login
```shell
nxc smb darkcorp.htb -u 'victor.r' -p 'victor1gustavo@#' --shares 
```