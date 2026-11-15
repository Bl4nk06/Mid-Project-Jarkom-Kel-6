# Kumpulan Skrip CLI VLSM OSPF

## 1. Skrip Migrasi OSPF — Router Core & Backbone

### 1.1 Router `R-PUSAT` (`router-id 6.6.6.1`)



```cisco
enable
configure terminal

router ospf 1
 router-id 6.6.6.1
 passive-interface default
 no passive-interface Serial0/0/1
 no passive-interface Serial0/1/0
 no passive-interface Serial0/1/1
 no passive-interface Serial0/2/0
 no passive-interface Serial0/2/1
 network 10.48.224.0 0.0.7.255 area 0
 network 10.48.241.0 0.0.0.31 area 0
 network 10.48.241.32 0.0.0.3 area 0
 network 10.48.241.36 0.0.0.3 area 0
 network 10.48.241.40 0.0.0.3 area 0
 network 10.48.241.44 0.0.0.3 area 0
 network 10.48.241.48 0.0.0.3 area 0
 default-information originate
exit

no ip route 10.48.96.0 255.255.224.0 10.48.241.34
no ip route 10.48.239.0 255.255.255.0 10.48.241.34
no ip route 10.48.192.0 255.255.224.0 10.48.241.50
no ip route 10.48.238.0 255.255.255.0 10.48.241.50
no ip route 10.48.128.0 255.255.224.0 10.48.241.46
no ip route 10.48.232.0 255.255.252.0 10.48.241.46
no ip route 10.48.64.0 255.255.224.0 10.48.241.42
no ip route 10.48.240.128 255.255.255.128 10.48.241.42
no ip route 10.48.0.0 255.255.192.0 10.48.241.38
no ip route 10.48.236.0 255.255.254.0 10.48.241.38
no ip route 10.48.241.52 255.255.255.252 10.48.241.38

end
write memory

```

### 1.2 Router `R-UTARA` (`router-id 6.6.6.3` + Link L7)



```cisco
enable
configure terminal

interface Serial0/1/0
 description LINK-L7-KE-R-DC
 ip address 10.48.241.58 255.255.255.252
 no shutdown
exit

router ospf 1
 router-id 6.6.6.3
 passive-interface default
 no passive-interface Serial0/0/0
 no passive-interface Serial0/0/1
 no passive-interface Serial0/1/0
 network 10.48.0.0 0.0.63.255 area 0
 network 10.48.236.0 0.0.1.255 area 0
 network 10.48.241.36 0.0.0.3 area 0
 network 10.48.241.52 0.0.0.3 area 0
 network 10.48.241.56 0.0.0.3 area 0
exit

no ip route 0.0.0.0 0.0.0.0 10.48.241.37
no ip route 10.48.160.0 255.255.224.0 10.48.241.54
no ip route 10.48.240.0 255.255.255.128 10.48.241.54

end
write memory

```

### 1.3 Router `R-PELABUHAN` (`router-id 6.6.6.4`)



```cisco
enable
configure terminal

router ospf 1
 router-id 6.6.6.4
 passive-interface default
 no passive-interface Serial0/0/0
 network 10.48.160.0 0.0.31.255 area 0
 network 10.48.240.0 0.0.0.127 area 0
 network 10.48.241.52 0.0.0.3 area 0
exit

no ip route 0.0.0.0 0.0.0.0 10.48.241.53

end
write memory

```

---

## 2. Skrip Migrasi OSPF — Router Cabang & DC

### 2.1 Router `R-DC` (`router-id 6.6.6.2` + Link L7)

```cisco
enable
configure terminal

no ip route 0.0.0.0 0.0.0.0 10.48.241.33

interface Serial0/0/1
 description LINK-L7-KE-R-UTARA
 ip address 10.48.241.57 255.255.255.252
 clock rate 64000
 no shutdown
exit

router ospf 1
 router-id 6.6.6.2
 passive-interface default
 no passive-interface Serial0/0/0
 no passive-interface Serial0/0/1
 network 10.48.96.0 0.0.31.255 area 0
 network 10.48.239.0 0.0.0.255 area 0
 network 10.48.241.32 0.0.0.3 area 0
 network 10.48.241.56 0.0.0.3 area 0
exit

end
write memory

```

### 2.2 Router `R-BARAT` (`router-id 6.6.6.5`)

```cisco
enable
configure terminal

no ip route 0.0.0.0 0.0.0.0 10.48.241.49

router ospf 1
 router-id 6.6.6.5
 passive-interface default
 no passive-interface Serial0/0/0
 network 10.48.192.0 0.0.31.255 area 0
 network 10.48.238.0 0.0.0.255 area 0
 network 10.48.241.48 0.0.0.3 area 0
exit

end
write memory

```

### 2.3 Router `R-TIMUR` (`router-id 6.6.6.6`)

```cisco
enable
configure terminal

no ip route 0.0.0.0 0.0.0.0 10.48.241.45

router ospf 1
 router-id 6.6.6.6
 passive-interface default
 no passive-interface Serial0/0/0
 network 10.48.128.0 0.0.31.255 area 0
 network 10.48.232.0 0.0.3.255 area 0
 network 10.48.241.44 0.0.0.3 area 0
exit

end
write memory

```

### 2.4 Router `R-SELATAN` (`router-id 6.6.6.7`)

```cisco
enable
configure terminal

no ip route 0.0.0.0 0.0.0.0 10.48.241.41

router ospf 1
 router-id 6.6.6.7
 passive-interface default
 no passive-interface Serial0/0/0
 network 10.48.64.0 0.0.31.255 area 0
 network 10.48.240.128 0.0.0.127 area 0
 network 10.48.241.40 0.0.0.3 area 0
exit

end
write memory

```