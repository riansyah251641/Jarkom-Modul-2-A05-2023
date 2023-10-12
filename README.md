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
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/04.png)

kemudian dibuat konfigurasi jaringan

- pandudewanata
  ```sh
  auto eth0
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
	netmask 255.255.255.0
 ```

 - DNS Master Yudhistira
```sh
auto eth0
iface eth0 inet static
	address 192.171.1.3
	netmask 255.255.255.0
	gateway 192.171.1.1
```

- DNS SLAVE Werkudara
```sh
auto eth0
iface eth0 inet static
	address 192.171.1.2
	netmask 255.255.255.0
	gateway 192.171.1.1
```

- Nakula
```sh
auto eth0
iface eth0 inet static
	address 192.171.2.2
	netmask 255.255.255.0
	gateway 192.171.2.1
```

- Sadewa
```sh
auto eth0
iface eth0 inet static
	address 192.171.2.3
	netmask 255.255.255.0
	gateway 192.171.2.1
```

- Abimanyu
```sh
auto eth0
iface eth0 inet static
	address 192.171.3.2
	netmask 255.255.255.0
	gateway 192.171.3.1
```

- Prabukusuma
```sh
auto eth0
iface eth0 inet static
	address 192.171.3.3
	netmask 255.255.255.0
	gateway 192.171.3.1
```

- Wisangeni
```sh
auto eth0
iface eth0 inet static
	address 192.171.3.4
	netmask 255.255.255.0
	gateway 192.171.3.1
```

- Arjuna (Load Balance)
```sh
auto eth0
iface eth0 inet static
	address 192.171.3.5
	netmask 255.255.255.0
	gateway 192.171.3.1
```

## No 2
Buatlah website utama pada node arjuna dengan akses ke arjuna.a05.com dengan alias www.arjuna.a05.com

masuk pada node arjuna
```sh
echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
   apt-get install bind9 -y
```
lalu buka
``` sh
nano /etc/bind/named.conf.local
```

masukkan

```sh
zone "arjuna.a05.com" {
	type master;
	file "/etc/bind/arjuna/arjuna.a05.com";
};
```

lalu buar direkotri bernama ```arjuna``` dan lakukan cp db.local
```sh
mkdir /etc/bind/arjuna
cp /etc/bind/db.local /etc/bind/arjuna/arjuna.a05.com
nano /etc/bind/arjuna/arjuna.a05.com
```

kemudian masuk kedalam file arjuna.a05.com dan setting menjadi seperti berikut
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20144429.png)

lalu ketik ```service bind9 restart```

untuk mencek masuk ke nakula client
hubungkan ke server arjuna
``` echo nameserver 192.171.3.5 > /etc/resolv.conf```
kemudian lakukan ping ``` ping arjuna.a05.com -c 5``` atau ``` host -t A arjuna.a05.com ```
maka akan muncul gambar berikut
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20144746.png)

## No 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

buka pada node yudhistira
```sh
echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
   apt-get install bind9 -y
```

lalu buka
``` sh
nano /etc/bind/named.conf.local
```
masukkan

```sh
zone "abimanyu.a05.com" {
	type master;
	file "/etc/bind/abimanyu/abimanyu.a05.com";
};
```

lalu buat direkotri bernama ```abimanyu``` dan lakukan cp db.local
```sh
mkdir /etc/bind/abimanyu
cp /etc/bind/db.local /etc/bind/abimanyu/abimanyu.a05.com
nano /etc/bind/abimanyu/abimanyu.a05.com
```

kemudian masuk kedalam file abimanyu.a05.com dan setting menjadi seperti berikut
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20145552.png)

lalu ketik ```service bind9 restart```

untuk mencek masuk ke nakula client
hubungkan ke server yudhistira
``` echo nameserver 192.171.1.3 > /etc/resolv.conf```
kemudian lakukan ping ``` ping abimanyu.a05.com -c 5``` atau ``` host -t A abimanyu.a05.com ```

## No 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

ketik
```nano /etc/bind/abimanyu/abimanyu.a05.com```

lalu tambahkan seperti digambar
![]()

lalu ketik ```service bind9 restart```

untuk mencek masuk ke nakula client
hubungkan ke server yudhistira
``` echo nameserver 192.171.1.3 > /etc/resolv.conf```
kemudian lakukan ping ``` ping parikesit.abimanyu.a05.com -c 5``` 

## No 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

pada node abimanyu buka

``` sh
nano /etc/bind/named.conf.local
```
masukkan

```sh
zone "1.171.192.in-addr.arpa" {
	type master;
	file "/etc/bind/abimanyu/1.171.192.in-addr.arpa";
};
```

lalu buat direkotri bernama ```1.171.192.in-addr.arpa``` dan lakukan cp db.local
```sh
mkdir /etc/bind/1.171.192.in-addr.arpa
cp /etc/bind/db.local /etc/bind/abimanyu/1.171.192.in-addr.arpa
nano /etc/bind/abimanyu/1.171.192.in-addr.arpa
```

lalu ubah seperti digambar
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20150350.png)

untuk mencek masuk ke nakula client
hubungkan ke server pusat
```sh
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils

//Kembalikan nameserver
host -t PTR 192.171.1.3
```

## No 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

masuk ke 
```nano /etc/bind/named.conf.local```

tambahkan di zone abimanyu.a05.com

``` allow-transfer { 192.171.1.2; }; ```
lalu restart bind

kemudian bukak di node werkudara
```sh
echo nameserver 192.168.122.1 > /etc/resolv.conf
  apt-get update
   apt-get install bind9 -y
```
lalu buka
``` sh
nano /etc/bind/named.conf.local
```

kemudian masukkan

```sh
zone "abimanyu.a05.com" {
    type slave;
    masters { 192.171.1.3; };
    file "/var/lib/bind/abimanyu.a05.com";
};

```

untuk cek matikan bind di abimanyu ```service bind9 stop```, lalu mencek masuk ke nakula client dengan

masuk ke ```nano /etc/resolf.conf```

tambahkan ```nameserver 192.171.1.2``` di bawahnya

kemudian lakukan ping ``` ping abimanyu.a05.com -c 5``` 

## No 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.

masuk ke node yudistira

ketik
```nano /etc/bind/abimanyu/abimanyu.a05.com```

ubah menjadi seperti digambar
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20160748.png)

lalu buka ```nano /etc/bind/named.conf.options``` beri ```//``` pada ```dnssec-validation auto;``` dan tambahkan ```allow-query{any;};``` dibawahnya

kemudian restart bind

lalu buka pada node werkudara

lalu buka ```nano /etc/bind/named.conf.options``` beri ```//``` pada ```dnssec-validation auto;``` dan tambahkan ```allow-query{any;};``` dibawahnya

buka ```nano  /etc/bind/named.conf.local```

masukan
```sh
zone "baratayuda.abimanyu.a05.com" {
	type master;
	file "/etc/bind/baratayuda/baratayuda.abimanyu.a05.com";
};

```

lalu buat direktori baratayuda

```sh
mkdir /etc/bind/baratayuda
cp /etc/bind/db.local /etc/bind/baratayuda/baratayuda.abimanyu.a05.com
nano /etc/bind/baratayuda/baratayuda.abimanyu.a05.com
```

lalu setting seperti pada gambar dibawah
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20161157.png)

kemudian restart bind dan jalankan di server client

## no 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

buka node werkudara
``` nano /etc/bind/baratayuda/baratayuda.abimanyu.a05.com```

lalu setting seperti di gambar
![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20162348.png)

kemudian restart bind dan cekdi client server

![](https://github.com/riansyah251641/Jarkom-Modul-2-A05-2023/blob/main/gambar/Cuplikan%20layar%202023-10-12%20162419.png)

## No 11
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.a05.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.a05

buka node abimanyu
jalankan
```sh
nano nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install lynx
apt-get install apache2
apt-get install php
service apache2 start
```

lalu masuk ke ```cd /etc/apache2/sites-available```

```sh
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/abimanyu.a05.com.conf
mkdir /var/www/abimanyu.a05
nano /etc/apache2/sites-available/abimanyu.a05.com.conf
```
```sh
 <VirtualHost *:80>
    ServerName abimanyu.a05.com
    ServerAlias www.abimanyu.a05.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.a05
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
lalu di 
```sh
cd /var/www/abimanyu.a05
nano index.php

```


```sh
<?php
    echo "ini adalah index.php, abimanyu.a05.com";
?>
```

lalu jangan lupa di jalankan programnya

```sh
a2ensite abimanyu.a05.com.conf
service apache2 restart

```

kemudian dipanggil dengan menggunakan lynx

## no 12 
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

masuk kedalam file abimanyu.a05.com.conf

lalu tambahkan agar menjadi

```sh
<VirtualHost *:80>
    ServerName abimanyu.a05.com
    ServerAlias www.abimanyu.a05.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/abimanyu.a05

    Alias "/home" "/var/www/abimanyu.a05.com/index.php/home"
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
kemudian Lakukan restart apache2 dengan service apache2 restart
dan cek dengan menggunakan lynx

## no 13
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

lalu masuk ke ```cd /etc/apache2/sites-available```

```sh
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/parikesit.abimanyu.a05.com.conf
mkdir /var/www/parikesit.abimanyu.a05
nano /etc/apache2/sites-available/paraikesit.abimanyu.a05.com.conf
```

lalu dikonfigurasikan
```sh
<VirtualHost *:80>
    ServerName parikesit.abimanyu.a05.com
    ServerAlias www.parikesit.abimanyu.a05.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/parikesit.abimanyu.a05

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

lalu jangan lupa di jalankan programnya

```sh
a2ensite parikesit.abimanyu.a05.com.conf
service apache2 restart

```

kemudian dipanggil dengan menggunakan lynx


## no 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

pertama buat folder di ```/var/www/parikesit.abimanyu.a05/```

lalu 
```sh
mkdir public
mkdir secret
```

kemudian buka konfigurasi ```nano /etc/apache2/sites-available/parikesit.abimanyu.a05.com.conf```

dan tambahkan
```sh
<VirtualHost *:80>
    ServerName parikesit.abimanyu.a05.com
    ServerAlias www.parikesit.abimanyu.a05.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/parikesit.abimanyu.a05

    <Directory /var/www/parikesit.abimanyu.a05.com/public>
    Options +Indexes
</Directory>

<Directory /var/www/parikesit.abimanyu.a05.com/secret>
    Options -Indexes
    Order deny,allow
    Deny from all
</Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

kemudian lakukan apache restart dan cek hasilnya di lynx

## no 15
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

buat folder error pada /var/www/parikesit.abimanyu.a05

masukkan file 404.html dan 403.html

dengan isi sbb
404.html
```sh
<!DOCTYPE html>
<html>
<head>
    <title>404 Not Found</title>
</head>
<body>
    <h1>404 tidak ketemu</h1>
    <p>Halaman yang Anda cari tidak ditemukan.</p>
</body>
</html>

```

403.html

```sh
<!DOCTYPE html>
<html>
<head>
    <title>403 forbidden</title>
</head>
<body>
    <h1>403 access denial</h1>
    <p>anda tidak punya hak untuk amsuk disini brow</p>
</body>
</html>

```

kemudian masuk ke nano ```/etc/apache2/sites-available/parikesit.abimanyu.a05.com.conf```
lalu tambahkan
```sh
ErrorDocument 404 /error/404.html
ErrorDocument 403 /error/403.html
```

dan kemudian lakukan restart apache dan cek di lynx

## no 16
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi 
www.parikesit.abimanyu.yyy.com/js 

masuk ke ```/etc/apache2/sites-available/parikesit.abimanyu.a05.com.conf```

lalu tambahkan
```Alias "/js" "/var/www/parikesit.ambimanyu.a05.com/public/js"```

dan jangan lupa dibuat folder home pada ```/var/www/parikesit.ambimanyu.a05```
dan kemudian lakukan restart apache dan cek di lynx

## no 17
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

masuk ```nano /etc/apache2/sites-available/rjp.baratayuda.abimanyu.a05.com.conf```

konfigurasi
```sh
<VirtualHost *:14000 *:14400>
    ServerAdmin webmaster@rjp.baratayuda.abimanyu.a05.com
    ServerName rjp.baratayuda.abimanyu.a05.com
    DocumentRoot /var/www/rjp.baratayuda.a05
    # Konfigurasi lainnya
</VirtualHost>
```

aktifkan
```a2ensite rjp.baratayuda.abimanyu.a05.com.conf```

Tambahkan port yang akan di listen pada ```ports.conf```
```sh
Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>
```
dan kemudian lakukan restart apache dan cek di lynx
## no 18
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

masuk ke ```/etc/apache2/sites-available/rjp.baratayuda.abimanyu.a05.com.conf```

tambahkan konfigurasi

```sh
<VirtualHost *:80>
    ServerAdmin webmaster@rjp.baratayuda.abimanyu.a05.com
    ServerName rjp.baratayuda.abimanyu.a05.com
    DocumentRoot /var/www/rjp.baratayuda.abimanyu.a05

    <Directory /var/www/rjp.baratayuda.abimanyu.a05>
        Options Indexes FollowSymLinks
        AllowOverride None
        AuthType Basic
        AuthName "Area Terlindungi"
        AuthUserFile /etc/apache2/.htpasswd
        Require valid-user
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```
buat username "Wayang" dan password "baratayudaa05" menggunakan berkas .htpasswd

```sh
htpasswd -c /etc/apache2/.htpasswd Wayang

```

dan kemudian lakukan restart apache dan cek di lynx

## 19
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

buka ```nano /etc/apache2/sites-available/000-default.conf```

lakukan konfigurasi
```sh
<VirtualHost *:80>
    ServerName YourServerIP
    Redirect permanent / http://www.abimanyu.yyy.com/
</VirtualHost>
```
dan kemudian lakukan restart apache dan cek di lynx

## 20
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.





