**tools untuk memetakan jalur serangan di Active Directory** — visualisasi siapa punya akses ke apa, dan gimana caranya sampai ke Domain Admin.
Kapan mulai pake bloodhound?
==setelah punya credential atau akses ke domain, sekecil apapun itu==

dahulukan credential plaintext untuk generate bloodhound

# Colect data
dahulukan credential plaintext untuk generate bloodhound
```shell
bloodhound-python -u 'wallace.everette' -p 'Welcome2026@' -dc 'DC01.logging.htb' -d 'logging.htb'  --dns-tcp -ns 10.129.198.221 --dns-timeout 10 --zip -c All
```

# Mark "OWNED"
semua akun yang dimiliki, termasuk:
- user password
- TGT
- shell

# Cek Outbound Object Control
apa yang bisa Akun ini kontrol.

| ACL           | Attack                                                                                           |
| ------------- | ------------------------------------------------------------------------------------------------ |
| Write Owner   | can change the owner of a group or modify the access control list (ACL) Management for the group |
| Generic All   | Full Control                                                                                     |
| Generic Write | Shadow Credential Attack                                                                         |
