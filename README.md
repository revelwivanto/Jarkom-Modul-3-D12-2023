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
Register domain berupa riegel.canyon.a09.com untuk worker Laravel dan granz.channel.a09.com untuk worker PHP,
sehinnga jalankan command berikut (no1.sh) di DNS Server (Heiter):
```
echo 'zone "riegel.canyon.d12.com" {
    type master;
    file "/etc/bind/sites/riegel.canyon.d12.com";
};

zone "granz.channel.d12.com" {
    type master;
    file "/etc/bind/sites/granz.channel.d12.com";
};

zone "1.197.192.in-addr.arpa" {
    type master;
    file "/etc/bind/sites/1.197.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

mkdir -p /etc/bind/sites
cp /etc/bind/db.local /etc/bind/sites/riegel.canyon.d12.com
cp /etc/bind/db.local /etc/bind/sites/granz.channel.d12.com
cp /etc/bind/db.local /etc/bind/sites/1.197.192.in-addr.arpa

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.d12.com. root.riegel.canyon.d12.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      riegel.canyon.d12.com.
@       IN      A       192.197.4.5     ; IP Fern
www     IN      CNAME   riegel.canyon.d12.com.' > /etc/bind/sites/riegel.canyon.d12.com

echo '
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.d12.com. root.granz.channel.d12.com. (
                        2023111401      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      granz.channel.d12.com.
@       IN      A       192.197.3.5     ; IP Lugner
www     IN      CNAME   granz.channel.d12.com.' > /etc/bind/sites/granz.channel.d12.com

echo 'options {
      directory "/var/cache/bind";

      forwarders {
              192.168.122.1;
      };

      // dnssec-validation auto;
      allow-query{any;};
      auth-nxdomain no;    # conform to RFC1035
      listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 start
```

### Hasil
![Screenshot 2023-11-16 171713](https://github.com/revelwivanto/Jarkom-Modul-1-D12-2023/assets/116476269/8d7d51a7-93a3-4f1b-8d99-c2618788b599)

### Soal 2 - 5
Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80 (2)
Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168 (3)
Client mendapatkan DNS dari Heiter dan dapat terhubung dengan internet melalui DNS tersebut (4)
Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch3 selama 3 menit sedangkan pada client yang melalui Switch4 selama 12 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 96 menit (5)


```
echo 'subnet 192.197.1.0 netmask 255.255.255.0 {
}

subnet 192.197.2.0 netmask 255.255.255.0 {
}

subnet 192.197.3.0 netmask 255.255.255.0 {
    range 192.197.3.16 192.197.3.32;
    range 192.197.3.64 192.197.3.80;
    option routers 192.197.3.0;
    option broadcast-address 192.197.3.255;
    option domain-name-servers 192.197.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 192.197.4.0 netmask 255.255.255.0 {
    range 192.197.4.12 192.197.4.20;
    range 192.197.4.160 192.197.4.168;
    option routers 192.197.4.0;
    option broadcast-address 192.197.4.255;
    option domain-name-servers 192.197.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}

service isc-dhcp-server restart
```

Restart node Clients dengan sehinnga memiliki IP

![Screenshot 2023-11-16 172129](https://github.com/revelwivanto/Jarkom-Modul-1-D12-2023/assets/116476269/6dea8b54-e6e9-46d4-834a-9cdb7fb3f45f)






