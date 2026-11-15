# Kumpulan Skrip CLI VLSM

## 1. Router Core & Backbone (Role C)



### 1.1 Router `R-ISP`



```cisco
enable
configure terminal
hostname R-ISP

interface Serial0/0/0
 description WAN-KE-R-PUSAT
 ip address 203.0.113.26 255.255.255.252
 clock rate 64000
 no shutdown
exit

interface GigabitEthernet0/0
 description SERVER-INTERNET
 ip address 8.8.8.1 255.255.255.0
 no shutdown
exit

end
write memory

```

### 1.2 Router `R-PUSAT`



```cisco
enable
configure terminal
hostname R-PUSAT

! --- INTERFACE LAN BALAI KOTA ---
interface GigabitEthernet0/0
 description LAN-PEGAWAI-BALAIKOTA
 ip address 10.48.224.1 255.255.248.0
 ip nat inside
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-SERVER-BALAIKOTA
 ip address 10.48.241.1 255.255.255.224
 ip nat inside
 no shutdown
exit

! --- INTERFACE WAN & BACKBONE L1 - L5 ---
interface Serial0/0/0
 description WAN-KE-R-ISP
 ip address 203.0.113.25 255.255.255.252
 ip nat outside
 no shutdown
exit

interface Serial0/0/1
 description L1-KE-R-DC
 ip address 10.48.241.33 255.255.255.252
 clock rate 64000
 ip nat inside
 no shutdown
exit

interface Serial0/1/1
 description L2-KE-R-UTARA
 ip address 10.48.241.37 255.255.255.252
 clock rate 64000
 ip nat inside
 no shutdown
exit

interface Serial0/2/1
 description L3-KE-R-SELATAN
 ip address 10.48.241.41 255.255.255.252
 clock rate 64000
 ip nat inside
 no shutdown
exit

interface Serial0/2/0
 description L4-KE-R-TIMUR
 ip address 10.48.241.45 255.255.255.252
 clock rate 64000
 ip nat inside
 no shutdown
exit

interface Serial0/1/0
 description L5-KE-R-BARAT
 ip address 10.48.241.49 255.255.255.252
 clock rate 64000
 ip nat inside
 no shutdown
exit

! --- STATIC ROUTING ---
ip route 0.0.0.0 0.0.0.0 203.0.113.26
ip route 10.48.96.0 255.255.224.0 10.48.241.34
ip route 10.48.239.0 255.255.255.0 10.48.241.34
ip route 10.48.192.0 255.255.224.0 10.48.241.50
ip route 10.48.238.0 255.255.255.0 10.48.241.50
ip route 10.48.128.0 255.255.224.0 10.48.241.46
ip route 10.48.232.0 255.255.252.0 10.48.241.46
ip route 10.48.64.0 255.255.224.0 10.48.241.42
ip route 10.48.240.128 255.255.255.128 10.48.241.42
ip route 10.48.0.0 255.255.192.0 10.48.241.38
ip route 10.48.236.0 255.255.254.0 10.48.241.38
ip route 10.48.241.52 255.255.255.252 10.48.241.38

! --- NAT OVERLOAD (PAT) ---
access-list 1 permit 10.48.0.0 0.0.255.255
ip nat inside source list 1 interface Serial0/0/0 overload

! --- TUNNEL GRE KE R-PELABUHAN ---
interface Tunnel0
 description GRE-KE-R-PELABUHAN
 ip address 192.168.6.1 255.255.255.252
 tunnel source Serial0/1/1
 tunnel destination 10.48.241.54
 tunnel mode gre ip
 no shutdown
exit

ip route 10.48.160.0 255.255.224.0 192.168.6.2
ip route 10.48.240.0 255.255.255.128 192.168.6.2

! --- DHCP PEGAWAI BALAI KOTA ---
ip dhcp excluded-address 10.48.224.1 10.48.224.10

ip dhcp pool POOL-PEGAWAI-BK
 network 10.48.224.0 255.255.248.0
 default-router 10.48.224.1
 dns-server 10.48.239.2
exit

end
write memory

```

### 1.3 Router `R-UTARA`



```cisco
enable
configure terminal
hostname R-UTARA

interface GigabitEthernet0/0
 description LAN-WIFI-UTARA
 ip address 10.48.0.1 255.255.192.0
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-SENSOR-LALULINTAS
 ip address 10.48.236.1 255.255.254.0
 no shutdown
exit

interface Serial0/0/0
 description L2-KE-R-PUSAT
 ip address 10.48.241.38 255.255.255.252
 no shutdown
exit

interface Serial0/0/1
 description L6-KE-R-PELABUHAN
 ip address 10.48.241.53 255.255.255.252
 clock rate 64000
 no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.48.241.37
ip route 10.48.160.0 255.255.224.0 10.48.241.54
ip route 10.48.240.0 255.255.255.128 10.48.241.54

ip dhcp excluded-address 10.48.0.1 10.48.0.10
ip dhcp excluded-address 10.48.236.1 10.48.236.10

ip dhcp pool POOL-WIFI-UTARA
 network 10.48.0.0 255.255.192.0
 default-router 10.48.0.1
 dns-server 10.48.239.2
exit

ip dhcp pool POOL-SENSOR-UTARA
 network 10.48.236.0 255.255.254.0
 default-router 10.48.236.1
 dns-server 10.48.239.2
exit

end
write memory

```

### 1.4 Router `R-PELABUHAN`



```cisco
enable
configure terminal
hostname R-PELABUHAN

interface GigabitEthernet0/0
 description LAN-SCADA-PELABUHAN
 ip address 10.48.160.1 255.255.224.0
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-KANTOR-PELABUHAN
 ip address 10.48.240.1 255.255.255.128
 no shutdown
exit

interface Serial0/0/0
 description L6-KE-R-UTARA
 ip address 10.48.241.54 255.255.255.252
 no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.48.241.53

interface Tunnel0
 description GRE-KE-R-PUSAT
 ip address 192.168.6.2 255.255.255.252
 tunnel source Serial0/0/0
 tunnel destination 10.48.241.37
 tunnel mode gre ip
 no shutdown
exit

ip route 10.48.224.0 255.255.248.0 192.168.6.1
ip route 10.48.241.0 255.255.255.224 192.168.6.1

ip dhcp excluded-address 10.48.160.1 10.48.160.10
ip dhcp excluded-address 10.48.240.1 10.48.240.10

ip dhcp pool POOL-SCADA-PELABUHAN
 network 10.48.160.0 255.255.224.0
 default-router 10.48.160.1
 dns-server 10.48.239.2
exit

ip dhcp pool POOL-KANTOR-PELABUHAN
 network 10.48.240.0 255.255.255.128
 default-router 10.48.240.1
 dns-server 10.48.239.2
exit

end
write memory

```

---

## 2. Router Cabang & Data Center (Role B)

### 2.1 Router `R-DC` (Termasuk DHCP Relay ke `SRV-DC`)

```cisco
enable
configure terminal
hostname R-DC

interface GigabitEthernet0/0
 description LAN-CCTV-KOTA
 ip address 10.48.96.1 255.255.224.0
 ip helper-address 10.48.239.2
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-STORAGE-DC
 ip address 10.48.239.1 255.255.255.0
 no shutdown
exit

interface Serial0/0/0
 description LINK-L1-KE-R-PUSAT
 ip address 10.48.241.34 255.255.255.252
 no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.48.241.33

ip dhcp excluded-address 10.48.239.1 10.48.239.10

ip dhcp pool POOL-STORAGE-DC
 network 10.48.239.0 255.255.255.0
 default-router 10.48.239.1
 dns-server 10.48.239.2
exit

end
write memory

```

### 2.2 Router `R-BARAT`

```cisco
enable
configure terminal
hostname R-BARAT

interface GigabitEthernet0/0
 description LAN-WIFI-BARAT
 ip address 10.48.192.1 255.255.224.0
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-PUSKESMAS-BARAT
 ip address 10.48.238.1 255.255.255.0
 no shutdown
exit

interface Serial0/0/0
 description LINK-L5-KE-R-PUSAT
 ip address 10.48.241.50 255.255.255.252
 no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.48.241.49

ip dhcp excluded-address 10.48.192.1 10.48.192.10
ip dhcp excluded-address 10.48.238.1 10.48.238.10

ip dhcp pool POOL-WIFI-BARAT
 network 10.48.192.0 255.255.224.0
 default-router 10.48.192.1
 dns-server 10.48.239.2
exit

ip dhcp pool POOL-PUSKESMAS-BARAT
 network 10.48.238.0 255.255.255.0
 default-router 10.48.238.1
 dns-server 10.48.239.2
exit

end
write memory

```

### 2.3 Router `R-TIMUR`

```cisco
enable
configure terminal
hostname R-TIMUR

interface GigabitEthernet0/0
 description LAN-WIFI-TIMUR
 ip address 10.48.128.1 255.255.224.0
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-SENSOR-BANJIR-TIMUR
 ip address 10.48.232.1 255.255.252.0
 no shutdown
exit

interface Serial0/0/0
 description LINK-L4-KE-R-PUSAT
 ip address 10.48.241.46 255.255.255.252
 no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.48.241.45

ip dhcp excluded-address 10.48.128.1 10.48.128.10
ip dhcp excluded-address 10.48.232.1 10.48.232.10

ip dhcp pool POOL-WIFI-TIMUR
 network 10.48.128.0 255.255.224.0
 default-router 10.48.128.1
 dns-server 10.48.239.2
exit

ip dhcp pool POOL-SENSOR-TIMUR
 network 10.48.232.0 255.255.252.0
 default-router 10.48.232.1
 dns-server 10.48.239.2
exit

end
write memory

```

### 2.4 Router `R-SELATAN`

```cisco
enable
configure terminal
hostname R-SELATAN

interface GigabitEthernet0/0
 description LAN-WIFI-SELATAN
 ip address 10.48.64.1 255.255.224.0
 no shutdown
exit

interface GigabitEthernet0/1
 description LAN-PJU-SELATAN
 ip address 10.48.240.129 255.255.255.128
 no shutdown
exit

interface Serial0/0/0
 description LINK-L3-KE-R-PUSAT
 ip address 10.48.241.42 255.255.255.252
 no shutdown
exit

ip route 0.0.0.0 0.0.0.0 10.48.241.41

ip dhcp excluded-address 10.48.64.1 10.48.64.10
ip dhcp excluded-address 10.48.240.129 10.48.240.138

ip dhcp pool POOL-WIFI-SELATAN
 network 10.48.64.0 255.255.224.0
 default-router 10.48.64.1
 dns-server 10.48.239.2
exit

ip dhcp pool POOL-PJU-SELATAN
 network 10.48.240.128 255.255.255.128
 default-router 10.48.240.129
 dns-server 10.48.239.2
exit

end
write memory

```

---

## 3. Ringkasan Konfigurasi GUI Server & Client (Pelengkap Bagian A)

* **`SRV-8.8.8.8` (Static):** IP `8.8.8.8`, Mask `255.255.255.0`, Gateway `8.8.8.1`.


* **`SRV-DC` (Static):** IP `10.48.239.2`, Mask `255.255.255.0`, Gateway `10.48.239.1`, DNS `10.48.239.2`. Layanan aktif: **DHCP** (`serverPool`: Gateway `10.48.96.1`, DNS `10.48.239.2`, Start IP `10.48.96.10`, Mask `255.255.224.0`, Max User `6000`), **DNS** (`A Record` untuk `k6.smartcity.lab` $\rightarrow$ `10.48.241.2`), dan **FTP** (user `cisco` / pass `cisco`).


* **`SRV-BALAIKOTA` (Static):** IP `10.48.241.2`, Mask `255.255.255.224`, Gateway `10.48.241.1`, DNS `10.48.239.2`. Layanan aktif: **HTTP/HTTPS** (`index.html` Portal Smart City Surabaya) dan **EMAIL** (domain `k6.smartcity.lab`, akun `pegawai` dan `pelabuhan`).


* **Ke-12 Perangkat Pengguna:** Seluruhnya diatur pada mode **DHCP** dan menarik konfigurasi IP serta DNS `10.48.239.2` secara otomatis.