Windows Server Update Service berfungsi sebagai server update lokal di dalam jaringan internal, sehingga client windows tidak usah langsung download dari microsoft update, tapi fetch saja dari WSUS server internal.

Secara default berjalan via **HTTP** port 8530
bukan di HTTPS port 8531, kecuali diaktifkan admin