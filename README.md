# Jarkom-Modul-3-E01-2021

- Clarence 05111940000104
- Nur Putra Khanafi 05111940000020
- Husnan 05111940007002

Script dan config" secara lengkap dapat dilihat pada repo https://github.com/theodorusclarence/jarkom-shift3-gns

### List

1. [Soal 1](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#1)
2. [Soal 2](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#2)
3. [Soal 3](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#3)
4. [Soal 4](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#4)
5. [Soal 5](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#5)
6. [Soal 6](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#6)
7. [Soal 7](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#7)
8. [Soal 8](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#8)
9. [Soal 9](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#9)
10. [Soal 10](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#10)
11. [Soal 11](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#11)
12. [Soal 12](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#12)
13. [Soal 13](https://github.com/nurputrak/Jarkom-Modul-3-E01-2021#13)

## 1

> Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server.

Enies → bind9

Jipangu → isc-dhcp-server

Water7 → squid

Maka, kita dapat menginstall 1 1 kebutuhan pada node"

```shell
apt-get update

apt-get install bind9 -y

apt-get install isc-dhcp-server -y

apt-get install squid -y
```

**Script**
```bash
# Jipangu
apt-get update
apt-get install isc-dhcp-server -y

cp -r shift3/default /etc/
cp -r shift3/dhcp /etc/

service isc-dhcp-server start

# Water7
apt-get update
apt-get install squid -y

# EniesLobby
apt-get update
apt-get install bind9 -y
```

**Pada Jipangu**
Ubah config interfaces di isc-dhcp-server, tambahkan eth0.
```bash
INTERFACES="eth0"
```

## 2

> Menjadikan Foosha sebagai DHCP Relay.

Tambahkan script untuk menginstall relay pada foosha.

```bash
apt-get update
apt-get install isc-dhcp-relay -y

cp -r shift3/default /etc/
cp -r shift3/network /etc/

service isc-dhcp-relay start
```

kemudian ubah config pada relay /etc/default/isc-dhcp-relay dengan interfaces ke eth yang ingin direlay.

```bash
# What servers should the DHCP relay forward requests to?
SERVERS="192.200.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

Server mengarah ke jipangu karena berlaku sebagai relay, dan Interfaces ke eth1 2 3 yang merupakan switch 1 2 3.

## 3-6

> Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.

Edit network config Alabasta, Loguetown, Skypie, Tottoland menjadi

```bash
auto eth0
iface eth0 inet dhcp
```

## 3

> Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169.

Edit konfigurasi.

```bash
subnet 192.200.1.0 netmask 255.255.255.0 {
    range 192.200.1.20 192.200.1.99;
    range 192.200.1.150 192.200.1.169;
    option routers 192.200.1.1;
    option broadcast-address 192.200.1.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 192.200.3.0 netmask 255.255.255.0 {
    range 192.200.3.30 192.200.3.50;
    option routers 192.200.3.1;
    option broadcast-address 192.200.3.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}

subnet 192.200.2.0 netmask 255.255.255.0 {
        option routers 192.200.2.1;
}
```

## 4

> Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50.

Edit konfigurasi.

```bash
subnet 192.200.1.0 netmask 255.255.255.0 {
    range 192.200.1.20 192.200.1.99;
    range 192.200.1.150 192.200.1.169;
    option routers 192.200.1.1;
    option broadcast-address 192.200.1.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 192.200.3.0 netmask 255.255.255.0 {
    range 192.200.3.30 192.200.3.50;
    option routers 192.200.3.1;
    option broadcast-address 192.200.3.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}

subnet 192.200.2.0 netmask 255.255.255.0 {
        option routers 192.200.2.1;
}
```

## 5

> Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

Edit konfigurasi.

```bash
subnet 192.200.1.0 netmask 255.255.255.0 {
    range 192.200.1.20 192.200.1.99;
    range 192.200.1.150 192.200.1.169;
    option routers 192.200.1.1;
    option broadcast-address 192.200.1.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 192.200.3.0 netmask 255.255.255.0 {
    range 192.200.3.30 192.200.3.50;
    option routers 192.200.3.1;
    option broadcast-address 192.200.3.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}

subnet 192.200.2.0 netmask 255.255.255.0 {
        option routers 192.200.2.1;
}
```

## 6

> Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

Edit konfigurasi.

```bash
subnet 192.200.1.0 netmask 255.255.255.0 {
    range 192.200.1.20 192.200.1.99;
    range 192.200.1.150 192.200.1.169;
    option routers 192.200.1.1;
    option broadcast-address 192.200.1.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 192.200.3.0 netmask 255.255.255.0 {
    range 192.200.3.30 192.200.3.50;
    option routers 192.200.3.1;
    option broadcast-address 192.200.3.255;
    option domain-name-servers 192.200.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}

subnet 192.200.2.0 netmask 255.255.255.0 {
        option routers 192.200.2.1;
}
```

## 7

> Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69.

--

## 8

> Loguetown digunakan sebagai client Proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi. Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000.

Pada squid.conf di water7, tambahkan

```bash
http_port 5000
visible_hostname jualbelikapal.e01.com
```

Kemudian nyalakan squid

## 9

> Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy. (yyy nama kelompok yaitu e01)

Pada water7, kita buat menggunakan htpasswd

```bash
touch /etc/squid/passwd

htpasswd -m /etc/squid/passwd luffybelikapale01
luffy_e01
luffy_e01

htpasswd -m /etc/squid/passwd zorobelikapale01
zoro_e01
zoro_e01
```

Kemudian tambahkan config squidnya


```bash
http_port 5000
visible_hostname jualbelikapal.e01.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

## 10

> Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

Kita dapat menambahkan untuk sebaliknya, yaitu dengan mendeny jam di luar, berikut rincian pembagian jam selain, dari yang satuan hingga gabungan, pada acl.conf

```bash
# Yang boleh
acl AVAILABLE_WORKING_1 time MTWH 07:00-11:00
acl AVAILABLE_WORKING_2 time TWHF 17:00-24:00
acl AVAILABLE_WORKING_3 time WHFA 00:00-03:00

# Dibalik per hari
acl AVAILABLE_WORKING time S 00:00-23:59

acl AVAILABLE_WORKING time M 00:00-06:59
acl AVAILABLE_WORKING time M 11:01-23:59

acl AVAILABLE_WORKING time T 00:00-06:59
acl AVAILABLE_WORKING time T 11:01-16:59

acl AVAILABLE_WORKING time W 11:01-16:59
acl AVAILABLE_WORKING time W 03:01-06:59

acl AVAILABLE_WORKING time H 11:01-16:59
acl AVAILABLE_WORKING time H 03:01-06:59

acl AVAILABLE_WORKING time F 03:01-16:59

acl AVAILABLE_WORKING time A 03:01-23:59

==== code atas gamasuk, hanya untuk penjelasan ====
==== yang ditambahin ke conf, dibawah ini aja  ====
# Digabung biar ga redundant
acl AVAILABLE_WORKING_1 time S 00:00-23:59
acl AVAILABLE_WORKING_2 time MT 00:00-06:59
acl AVAILABLE_WORKING_3 time M 11:01-23:59
acl AVAILABLE_WORKING_4 time TWH 11:01-16:59
acl AVAILABLE_WORKING_5 time WH 03:01-06:59
acl AVAILABLE_WORKING_6 time F 03:01-16:59
acl AVAILABLE_WORKING_7 time A 03:01-23:59
```

Kemudian tambahkan pada config squidnya, dengan mendeny jam" diluar available.

```bash
include /etc/squid/acl.conf

http_port 5000
visible_hostname jualbelikapal.e01.com

http_access deny AVAILABLE_WORKING_1
http_access deny AVAILABLE_WORKING_2
http_access deny AVAILABLE_WORKING_3
http_access deny AVAILABLE_WORKING_4
http_access deny AVAILABLE_WORKING_5
http_access deny AVAILABLE_WORKING_6
http_access deny AVAILABLE_WORKING_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

## 11

> Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie.

Pada EniesLobby, lakukan config DNS terlebih dahulu, dengan menggunakan file" yang sama pada shift2.

```bash
# /etc/bind/named.conf.local

zone "super.franky.e01.com" {
    type master;
    file "/etc/bind/kaizoku/super.franky.e01.com";
    allow-transfer { 192.200.3.69; }; // Masukan IP Skypie tanpa tanda petik
};
```

```bash
# /etc/bind/kaizoku/super.franky.e01.com
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     super.franky.e01.com. root.super.franky.e01.com. (
                        2021110801      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      super.franky.e01.com.
@       IN      A       192.200.3.69
www     IN      CNAME   super.franky.e01.com.
```

Kemudian pada Skypie, tambahkan file html ke /var/www/super.franky.e01.com

```bash
# /etc/apache2/apache2.conf

# Tambahkan paling bawah
ServerName 192.200.3.69
```

```bash
# /etc/apache2/sites-available/super.franky.e01.com.conf
<VirtualHost *:80>
    ServerName super.franky.e01.com
    ServerAlias www.super.franky.e01.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/super.franky.e01.com

    <Directory /var/www/super.franky.e01.com>
        Options +Indexes
        AllowOverride All
    </Directory>

    <Directory /var/www/super.franky.e01.com/public>
        Options +Indexes
    </Directory>

    Alias "/js" "/var/www/super.franky.e01.com/public/js"

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    ErrorDocument 404 /error/404.html
</VirtualHost>
```

Pada Water7, ditambahkan nameserver mengarah ke enieslobby.

## 12 & 13

> Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps **(12)** Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya **(13)**

```bash
# acl-bandwidth.conf
acl download url_regex -i \.jpg$ \.png$

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

acl luffy proxy_auth luffybelikapale01
acl zoro proxy_auth zorobelikapale01

delay_pools 2
delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 deny zoro
delay_access 1 allow download
delay_access 1 deny all

delay_class 2 1
delay_parameters 2 -1/-1
delay_access 2 allow zoro
delay_access 2 deny luffy
delay_access 2 deny all
```

Pada config ini, kita mengambil 3 acl yaitu:
* download → file berekstensi .jpg dan .png
* luffy → proxy_auth luffybelikapale01
* zoro → proxy_auth zorobelikapale01

Dengan acl tersebut, kita bisa mengalokasikan pada delay pools.
Pada delay class pertama, kita menambahkan 1250bytes → 10.000bit (10Kb) dengan threshold 1250bytes. Kemudian yang di allow adalah luffy dan download, sisanya di deny.
Pada delay class kedua, kita menambahkan parameter -1/-1, yang berarti tidak ada bandwidth, dan mengallow zoro, dan mendeny luffy.

```bash
include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

http_port 5000
visible_hostname jualbelikapal.e01.com

http_access deny AVAILABLE_WORKING_1
http_access deny AVAILABLE_WORKING_2
http_access deny AVAILABLE_WORKING_3
http_access deny AVAILABLE_WORKING_4
http_access deny AVAILABLE_WORKING_5
http_access deny AVAILABLE_WORKING_6
http_access deny AVAILABLE_WORKING_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

acl BLACKLIST dstdomain google.com
deny_info http://super.franky.e01.com/ BLACKLIST
```
Kemudian config terpisah dapat ditambahkan

Karena sulit untuk mengecek yang -1, maka saya memberi screenshot dengan membatasi 100byte. Untuk contoh tidak dibandwidth.
![tidakdibandwidth](https://user-images.githubusercontent.com/57633103/141646057-de5b500b-1ec1-47d2-b5d3-431a2933f5df.png)

## Script.sh

```bash
# Jipangu
apt-get update
apt-get install isc-dhcp-server -y

cp -r shift3/default /etc/
cp -r shift3/dhcp /etc/

service isc-dhcp-server start

# Foosha
apt-get update
apt-get install isc-dhcp-relay -y

cp -r shift3/default /etc/
cp -r shift3/network /etc/

service isc-dhcp-relay start

# Skypie
pt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y

cp -r shift3/www /var/
cp -r shift3/sites-available /etc/apache2

cd /etc/apache2/sites-available
a2ensite super.franky.e01.com.conf

service apache2 start

# Water7
apt-get update
apt-get install squid -y
apt-get install apache2-utils -y


cp -r shift3/squid /etc/

echo "nameserver 192.200.2.2" > /etc/resolv.conf

service squid start

# EniesLobby
apt-get update
apt-get install bind9 -y

cp -r shift3/bind /etc/

service bind9 start

#Loguetown
apt-get update
apt-get install lynx -y
apt-get install speedtest-cli -y

## 2 ini harus dirun sendiri
export PYTHONHTTPSVERIFY=0
export http_proxy=http://192.200.2.3:5000
```

### Kendala Pengerjaan

1. Pada saat mencoba super.franky, kita lupa memasukkan nameserver enieslobby, sehingga tidak bisa connect ke super franky.
2. Kemudian untuk mencari cara proxy_auth, lumayan memakan waktu sehingga agak lama.
