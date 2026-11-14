### 1. Koneksi Backbone & Internet (Antar-Router & Server `8.8.8.8`)

| Link / Jalur | Node Asal | Port Asal | Jenis Kabel | Port Tujuan | Node Tujuan |
| --- | --- | --- | --- | --- | --- |
| **Internet** | `R-ISP` | `GigabitEthernet0/0` | Copper Crossover (`CCO`) | `FastEthernet0` | `SRV-8.8.8.8` |
| **WAN ISP** | `R-ISP` | `Serial0/0/0` | Serial DCE | `Serial0/0/0` | `R-PUSAT` |
| **Link L1** | `R-PUSAT` | `Serial0/0/1` | Serial DCE | `Serial0/0/0` | `R-DC` |
| **Link L5** | `R-PUSAT` | `Serial0/1/0` | Serial DCE | `Serial0/0/0` | `R-BARAT` |
| **Link L2** | `R-PUSAT` | `Serial0/1/1` | Serial DCE | `Serial0/0/0` | `R-UTARA` |
| **Link L4** | `R-PUSAT` | `Serial0/2/0` | Serial DCE | `Serial0/0/0` | `R-TIMUR` |
| **Link L3** | `R-PUSAT` | `Serial0/2/1` | Serial DCE | `Serial0/0/0` | `R-SELATAN` |
| **Link L6** | `R-UTARA` | `Serial0/0/1` | Serial DCE | `Serial0/0/0` | `R-PELABUHAN` |
| **Link L7** *(Cadangan OSPF - Soal 7)* | `R-DC` | `Serial0/0/1` | Serial DCE *(pasang di file OSPF)* | `Serial0/1/0` | `R-UTARA` |

---

### 2. Koneksi Jaringan Lokal (14 Segmen LAN)

| Wilayah | Node Asal | Port Asal | Jenis Kabel / Media | Port Tujuan | Node Tujuan |
| --- | --- | --- | --- | --- | --- |
| **Balai Kota (`R-PUSAT`)** | `R-PUSAT` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-PEGAWAI-BK` |
|  | `SW-PEGAWAI-BK` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-PEGAWAI-BK` |
|  | `R-PUSAT` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-SRV-BK` |
|  | `SW-SRV-BK` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `SRV-BALAIKOTA` |
| **Command Center (`R-DC`)** | `R-DC` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-CCTV` |
|  | `SW-CCTV` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-CCTV-KOTA` |
|  | `R-DC` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-DC` |
|  | `SW-DC` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `SRV-DC` |
| **Surabaya Barat (`R-BARAT`)** | `R-BARAT` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-WIFI-BARAT` |
|  | `SW-WIFI-BARAT` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `Port 0` | `AP-BARAT` |
|  | `AP-BARAT` | `Port 1` *(Wireless)* | Wireless (2.4 GHz) | `Wireless0` | `HP-WIFI-BARAT` |
|  | `R-BARAT` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-PNS-B` |
|  | `SW-PNS-B` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-PNS-B` |
| **Surabaya Utara (`R-UTARA`)** | `R-UTARA` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-WIFI-UTARA` |
|  | `SW-WIFI-UTARA` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `Port 0` | `AP-UTARA` |
|  | `AP-UTARA` | `Port 1` *(Wireless)* | Wireless (2.4 GHz) | `Wireless0` | `HP-WIFI-UTARA` |
|  | `R-UTARA` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-SLU-UTARA` |
|  | `SW-SLU-UTARA` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-SLU-UTARA` |
| **Surabaya Timur (`R-TIMUR`)** | `R-TIMUR` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-WIFI-TIMUR` |
|  | `SW-WIFI-TIMUR` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `Port 0` | `AP-TIMUR` |
|  | `AP-TIMUR` | `Port 1` *(Wireless)* | Wireless (2.4 GHz) | `Wireless0` | `HP-WIFI-TIMUR` |
|  | `R-TIMUR` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-SB-TIMUR` |
|  | `SW-SB-TIMUR` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-SB-TIMUR` |
| **Surabaya Selatan (`R-SELATAN`)** | `R-SELATAN` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-WIFI-SELATAN` |
|  | `SW-WIFI-SELATAN` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `Port 0` | `AP-SELATAN` |
|  | `AP-SELATAN` | `Port 1` *(Wireless)* | Wireless (2.4 GHz) | `Wireless0` | `HP-WIFI-SELATAN` |
|  | `R-SELATAN` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-PJU-SELATAN` |
|  | `SW-PJU-SELATAN` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-PJU-SELATAN` |
| **Pelabuhan (`R-PELABUHAN`)** | `R-PELABUHAN` | `GigabitEthernet0/0` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-SCADA-PEL` |
|  | `SW-SCADA-PEL` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-SCADA-PEL` |
|  | `R-PELABUHAN` | `GigabitEthernet0/1` | Copper Straight-Through (`CST`) | `FastEthernet0/1` | `SW-KANTOR-PEL` |
|  | `SW-KANTOR-PEL` | `FastEthernet0/2` | Copper Straight-Through (`CST`) | `FastEthernet0` | `PC-KANTOR-PEL` |