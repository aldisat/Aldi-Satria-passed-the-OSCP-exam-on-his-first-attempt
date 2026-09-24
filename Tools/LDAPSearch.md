# 0. Probles
## a. TLS problem
![](Attachments/Pasted%20image%2020260910144511.png)
## b. Limit Exceede
![](Attachments/Pasted%20image%2020260911131113.png)
```shell
LDAPTLS_REQCERT=never ldapsearch -x -H ldaps://danglingtree.htb \
  -D 'jake.h@danglingtree.htb' -w 'NewPassword123' \
  -E pr=500/noprompt \
  -b "CN=Configuration,DC=danglingtree,DC=htb" \
  objectClass | grep -i '^objectClass:' | sort -u
```
![](Attachments/Pasted%20image%2020260911131217.png)
## c. No wrap
```shell
-o ldif-wrap=no 
```
# 1. Normal search without filter
```shell
# Untuk list user, group, dan yang bersifat normal, identity & access data
LDAPTLS_REQCERT=never ldapsearch -x -H ldaps://danglingtree.htb \
  -D 'jake.h@danglingtree.htb' -w 'NewPassword123' \
  -b "DC=danglingtree,DC=htb"

# Untuk list CA, konfigurasi, instructure
LDAPTLS_REQCERT=never ldapsearch -x -H ldaps://danglingtree.htb \
  -D 'jake.h@danglingtree.htb' -w 'NewPassword123' \
  -b "CN=Configuration,DC=danglingtree,DC=htb" 
```
![](Attachments/Pasted%20image%2020260911095831.png)

# 2. List objectClass
```shell
LDAPTLS_REQCERT=never ldapsearch -x -H ldaps://danglingtree.htb \
  -D 'jake.h@danglingtree.htb' -w 'NewPassword123' \
  -E pr=500/noprompt \
  -b "CN=Configuration,DC=danglingtree,DC=htb" \
  objectClass | grep -i '^objectClass:' | sort -u
```
# 3. List User
```shell
# nama Akun biasa
LDAPTLS_REQCERT=never ldapsearch -x -LLL -H ldaps://scaffold.htb \
  -D 'j.harris@scaffold.htb' -w 'Harr1sHelpdesk2026!Breach' \
  -b "DC=scaffold,DC=htb" \
  "(objectClass=user)" cn | grep 'cn:' | sed 's/cn\: //g' | tee users.txt
 
# SAM account name (PAKE YANG INI!!!)
LDAPTLS_REQCERT=never ldapsearch -x -LLL -H ldaps://scaffold.htb \
  -D 'j.harris@scaffold.htb' -w 'Harr1sHelpdesk2026!Breach' \
  -b "DC=scaffold,DC=htb" \
  "(objectClass=user)" sAMAccountName | grep 'sAMAccountName:' | sed 's/sAMAccountName\: //g' | tee SAM_users.txt
```
![](Attachments/Pasted%20image%2020260911105019.png)

# 4. List Groups
```shell
net rpc group list -U 'j.harris%Harr1sHelpdesk2026!Breach' -S scaffold.htb
```
![](Attachments/Pasted%20image%2020260918103447.png)
# 5. List OID
```shell
LDAPTLS_REQCERT=never ldapsearch -x \
  -H ldaps://10.129.39.163:636 \
  -D 'jake.h@danglingtree.htb' \
  -w 'NewPassword123' \
  -b 'CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' \
  '(objectClass=msPKI-Enterprise-Oid)' \
  msPKI-Cert-Template-OID displayName
```