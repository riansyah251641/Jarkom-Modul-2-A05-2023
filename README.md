# Jarkom-Modul-2-A05-2023
## Kelompok A05:
| Nama | NRP |
| ---------------------- | ---------- |
| Muhammad Ferbiansyah | 5025211164 |
| Layyinatul Fuadah | 5025211207 |

## No 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 

### jawab
dibuat dengan gns3 seperti bentuk di gambar
!(https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/04.png)

kemudian dibuat konfigurasi jaringan

- pandudewanata
  ```auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.171.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.171.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.171.3.1
	netmask 255.255.255.0```

 - DNS Master Yudhistira
```auto eth0
iface eth0 inet static
	address 192.171.1.3
	netmask 255.255.255.0
	gateway 192.171.1.1```

- DNS SLAVE Werkudara
```auto eth0
iface eth0 inet static
	address 192.171.1.2
	netmask 255.255.255.0
	gateway 192.171.1.1```

- Nakula
```auto eth0
iface eth0 inet static
	address 192.171.2.2
	netmask 255.255.255.0
	gateway 192.171.2.1```

- Sadewa
```auto eth0
iface eth0 inet static
	address 192.171.2.3
	netmask 255.255.255.0
	gateway 192.171.2.1```

- Abimanyu
```auto eth0
iface eth0 inet static
	address 192.171.3.2
	netmask 255.255.255.0
	gateway 192.171.3.1```

- Prabukusuma
```auto eth0
iface eth0 inet static
	address 192.171.3.3
	netmask 255.255.255.0
	gateway 192.171.3.1```

- Wisangeni
```auto eth0
iface eth0 inet static
	address 192.171.3.4
	netmask 255.255.255.0
	gateway 192.171.3.1```

- Arjuna (Load Balance)
```auto eth0
iface eth0 inet static
	address 192.171.3.5
	netmask 255.255.255.0
	gateway 192.171.3.1```

## No 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.a05.com dengan alias www.arjuna.a05.com

masuk pada node arjuna
```echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
   apt-get install bind9 -y```
