# Jarkom-Modul-3-D12-2023
Laporan Resmi Modul Jarkom 3 D12 2023
|Nama|NRP|
|-----|-----|
|Muhammad Revel Wivanto|5025211|
|Muhammad Choirun Ni'am|5025221203|

<img width="466" alt="peta_topologi" src="https://github.com/cchoirun/Jarkom-Modul-2-D12-2023/assets/115228631/d83b9271-9419-471c-bf01-a6ec1aec0fbc">

Pertama-tama, kita akan membuat konfigurasi sesuai topologi yang diberikan : 

### Aura
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.197.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.197.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.197.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 192.197.4.1
	netmask 255.255.255.0
```

### Himmel
```
auto eth0
iface eth0 inet static
	address 192.197.1.2
	netmask 255.255.255.0
	gateway 192.197.1.1
```

### Heiter
```
auto eth0
iface eth0 inet static
	address 192.197.1.3
	netmask 255.255.255.0
	gateway 192.197.1.1
```

### Denken
```
auto eth0
iface eth0 inet static
	address 192.197.2.2
	netmask 255.255.255.0
	gateway 192.197.2.1
```

### Eisen
```
auto eth0
iface eth0 inet static
	address 192.197.2.3
	netmask 255.255.255.0
	gateway 192.197.2.1
```

### Revolte
```
auto eth0
iface eth0 inet dhcp
```

### Richter
```
auto eth0
iface eth0 inet dhcp
```
### Lawine
```
auto eth0
iface eth0 inet static
	address 192.197.4.4
	netmask 255.255.255.0
	gateway 192.197.3.1
```

### Linie
```
auto eth0
iface eth0 inet static
	address 192.197.3.5
	netmask 255.255.255.0
	gateway 192.197.3.1
```

### Lugner
```
auto eth0
iface eth0 inet static
	address 192.197.3.6
	netmask 255.255.255.0
	gateway 192.197.3.1
```

### Sein
```
auto eth0
iface eth0 inet dhcp
```

### Stark
```
auto eth0
iface eth0 inet dhcp
```

### Frieren
```
auto eth0
iface eth0 inet static
	address 192.197.4.4
	netmask 255.255.255.0
	gateway 192.197.4.1
```

### Flamme
```
auto eth0
iface eth0 inet static
	address 192.197.4.5
	netmask 255.255.255.0
	gateway 192.197.4.1
```

### Fern
```
auto eth0
iface eth0 inet static
	address 192.197.4.6
	netmask 255.255.255.0
	gateway 192.197.4.1
```

### Selanjutnya kita configure untuk masing-masing node : 
Note : Pastikan untuk memasukkan command : ``` echo nameserver 192.168.122.1 > /etc/resolv.conf ``` sebelum menginstall sesuatu

### Aura
DHCP Relay 
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.197.0.0/16
echo nameserver 192.168.122.1 > /etc/resolve.conf
apt-get update
apt-get install isc-dhcp-relay
```
Lalu pada file ```etc/dhcp/dhcpd.conf```, kita dapat konfigurasi untuk forward ke IP DHCP server yakni Himmel dan menambahkan interfaces sesuai topologi yakni : ```eth 1 eth2 eth3 eth4``` dengan cara menambahkan kode berikut
```
SERVER = "192.197.1.2"
INTERFACES = "eth1 eth2 eth3 eth4"
OPTIONS = ""
```

### Himmel
DHCP Server
```
apt-get update
apt-get install isc-dhcp-server -y
```

### Heiter
DNS Server
```
apt-get update
apt-get install bind9 -y
```


### Soal 1
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server.
Agar semua client menggunakan konfigurasi IP dari DHCP Server, maka pada konfigurasi jaringan tiap node client kita tambahkan :
```
auto eth0
iface eth0 inet dhcp
```
Dimana node client adalah : 
<ul>
  <li>Revolte</li>
  <li>Richter</li>
  <li>Sein</li>
  <li>Stark</li>
</ul>

### Soal 2
Client yang melalui Switch3 mendapatkan range IP dari 192.197.3.16 - 192.197.3.32 dan 192.197.3.64 - 192.197.3.80






