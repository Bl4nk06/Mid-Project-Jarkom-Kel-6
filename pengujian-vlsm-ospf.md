# Pengujian VLSM OSPF

1. **Verifikasi Neighbor & Tabel Routing OSPF (di `R-PUSAT`)**:


```cisco
show ip ospf neighbor
show ip route ospf

```


Pastikan kelima tetangga langsung (`6.6.6.2`, `6.6.6.3`, `6.6.6.5`, `6.6.6.6`, `6.6.6.7`) berstatus **`FULL`** dan seluruh segmen jaringan cabang sudah terisi dengan kode **`O`**.


2. **Putuskan Link Utama `L1` (di `R-PUSAT`)**:


```cisco
enable
configure terminal
interface Serial0/0/1
shutdown
end

```


3. **Uji Perpindahan Jalur Otomatis (Failover)**:
* Di CLI **`R-PUSAT`**, jalankan `traceroute 10.48.241.57` (atau dari Command Prompt **`PC-PEGAWAI-BK`**, jalankan `tracert 10.48.239.2`).


* Jalur paket akan otomatis memutar lewat `R-UTARA` (`10.48.241.38`) $\rightarrow$ Link Cadangan `L7` (`10.48.241.57`) $\rightarrow$ `SRV-DC` (`10.48.239.2`).




4. **Pulihkan Kembali Link `L1` Setelah Uji (Wajib di `R-PUSAT`)**:


```cisco
enable
configure terminal
interface Serial0/0/1
no shutdown
end
write memory

```


Setelah neighbor `6.6.6.2` kembali **`FULL`**, tekan **Save** pada **`K6_VLSM_OSPF.pkt`**.