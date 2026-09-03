file dll yang biasanya menjadi core data aplikasi
```powershell
SmarterMail.Standard.dll
```

![](Attachments/Pasted%20image%2020260901144546.png) 

# DES/3DES
```shell
# 1. Nama class/method yang eksplisit nyebut cryptoa
grep -n "class.*[Cc]rypt\|class.*[Ee]ncrypt\|class.*[Cc]ipher\|class.*[Ss]ecur" 
grep -n "class.*[Pp]assword\|class.*[Hh]ash" 

# 2. Import/using statement yang nunjukkin algoritma dipakai
grep -n "using System.Security.Cryptography" 
grep -n "DES\.\|DES3\.\|TripleDES\.\|Aes\.\|RC2\.\|RC4\|Rijndael" 

# 3. Deklarasi array byte statis/const (paling penting!)
grep -n "byte\[\] key\|byte\[\] iv\|byte\[\] Key\|byte\[\] IV" 
grep -n "new byte\[8\]\|new byte\[16\]\|new byte\[24\]\|new byte\[32\]" 

# 4. String literal yang "terlihat kayak password/passphrase"
grep -n "PasswordDeriveBytes\|Rfc2898DeriveBytes\|DeriveBytes\|PBKDF2" 

# 5. Constant string yang aneh/random-looking
grep -n "private const string\|static.*string.*=.*\".*[\!@#\$%^&*].*\""

# 6. CreateEncryptor / CreateDecryptor — titik pemanggilan
grep -n "CreateEncryptor\|CreateDecryptor\|\.Encrypt(\|\.Decrypt("
```
![](Attachments/Pasted%20image%2020260902125917.png)

kalau menemuka 2 keymap, coba search keymap itu dipake paling banyak apa?    
gunakan vim untuk melihat secara keseluruhan
seach vim ignore cas -> \c atau set: ic
![](Attachments/Pasted%20image%2020260902140833.png)

decrypte
```shell
from Crypto.Cipher import DES
from Crypto.Util.Padding import unpad
import base64

# keymap2 — dari CryptographyHelper, gated oleh "a3oij89FF!apoife"
key = bytes([180,63,132,209,16,180,233,145])
iv  = bytes([1,216,174,230,73,173,146,39])

ciphertext_b64 = "66e7ppLOBF7UdzDv7zK6MJ1rmyUb1Cby"
ct = base64.b64decode(ciphertext_b64)

cipher = DES.new(key, DES.MODE_CBC, iv)
pt = unpad(cipher.decrypt(ct), DES.block_size)
print(pt.decode())
```
![](Attachments/Pasted%20image%2020260902141007.png)