# Pengujian VLSM

1. **Uji Konektivitas & Static Routing (dari `PC-PNS-B` $\rightarrow$ Command Prompt):**
* `ping 10.48.238.1` (Gateway `R-BARAT`)
* `ping 10.48.224.1` (Core `R-PUSAT`)
* `ping 10.48.239.2` (`SRV-DC`)
* `ping 10.48.241.2` (`SRV-BALAIKOTA`)
* `ping 10.48.240.11` (`PC-KANTOR-PEL` lintas cabang)


2. **Uji NAT Overload ke Internet:**
* Dari `PC-PNS-B`, jalankan `ping 8.8.8.8` (harus *Reply*).
* Buka CLI **`R-PUSAT`**, jalankan `show ip nat translations` $\rightarrow$ pastikan IP privat `10.48.238.x` diterjemahkan ke `203.0.113.25`.




3. **Uji DNS & Web Server:**
* Dari `PC-CCTV-KOTA` (atau PC mana saja), buka **Desktop $\rightarrow$ Web Browser**.
* Ketik **`[http://k6.smartcity.lab](http://k6.smartcity.lab)`** di kolom URL $\rightarrow$ klik **Go** $\rightarrow$ pastikan halaman HTML Portal Smart City muncul.


4. **Uji FTP Server (`SRV-DC`):**
* Dari Command Prompt PC mana saja, ketik `ftp 10.48.239.2` (atau `ftp dc.k6.smartcity.lab`).
* Masukkan Username **`cisco`** dan Password **`cisco`**, lalu ketik `dir` untuk melihat daftar file di server.


5. **Uji Email Server (`SRV-BALAIKOTA`):**
* Buka **Desktop $\rightarrow$ Email** di **`PC-PNS-B`**, isi: *Your Name* `Pegawai`, *Email Address* `pegawai@k6.smartcity.lab`, *Incoming & Outgoing Mail Server* `10.48.241.2`, *User Name* `pegawai`, *Password* `123` $\rightarrow$ klik **Save**.
* Lakukan hal yang sama di **`PC-KANTOR-PEL`** dengan akun `pelabuhan@k6.smartcity.lab` (user `pelabuhan`, pass `123`).
* Klik **Compose** di `PC-PNS-B`, kirim email ke `pelabuhan@k6.smartcity.lab`, lalu klik **Receive** di `PC-KANTOR-PEL`.


6. **Uji Tunnel GRE (dari `PC-PEGAWAI-BK` $\rightarrow$ Command Prompt):**
* Jalankan `tracert 10.48.240.11` $\rightarrow$ pastikan urutan *hop*-nya adalah `10.48.224.1` $\rightarrow$ **`192.168.6.2`** $\rightarrow$ `10.48.240.11` (tanpa melewati IP `R-UTARA`).