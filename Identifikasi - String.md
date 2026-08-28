
1. Cek magic bytes kalau itu file `file namafile` -> `xxd namafile | head`
2. Kalau string → coba tools identifikasi otomatis
3. Tentukan: Decode / Crack / Decrypt?

# 1. Cek validitas base64
1. Hitung jumlah karakter
2. Jika hanya A-Z a-z 0-9 + / = , panjang kelipatan 4 → kemungkinan Base64
3. Jika hanya 0-9 a-f → Hex
4. Jika panjang fix (32/40/64 hex char) → kemungkinan HASH
5. Random / punya magic bytes/header aneh di file → kemungkinan ENCRYPTED FILE