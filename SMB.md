

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

# Explore
cari sensitif file yang extensionnnya .config atau exe, cari terbuat dari apa lalu decompile file tersebut, grep password, kalau dapat credential gas login smb dengan creds itu

kalau SYSVOL dan NETLOGON pada SMB, dipastikan mesin tersebut adalah Domain Controller, cek share itu dulu.
```shell
#untuk null session
smbclient //10.129.244.81/<share> -N

#untuk authenticated session
smbclient //10.129.244.81/<share> -U <user>

#download semuanya
smbclient /10.129.244.81/<share> -U username 
smb: \> recurse ON
smb: \> prompt OFF
smb: \> mget *

#alternative kalau smblient tidak bisa
smbmap -H 10.129.245.130 -u 'wallace.everette' -p 'Welcome2026@' -r | tee smbmap.txt
smbmap -H 10.129.245.130 -u wallace.everette -p Welcome2026@ --download './Logs/IdentitySync_Trace_20260219.log'
```

# List User
```shell
# jika nama ada akhiran $, berarti itu adalah computer account  
nxc smb 10.129.245.130 -u 'wallace.everette' -p 'Welcome2026@' --rid-brute | tee listuser.txt
```