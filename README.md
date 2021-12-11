# Jarkom-Modul-5-A02-2021

## Angggota Kelompok A02:
- Naufaliando Yudo Kusumo - 05111940000169
- Kevin Davi Samuel - 05111940000157
- Muhamad Affan - 05111840000109

## A . Topologi :

[![img1.jpg](https://i.postimg.cc/qR88Yrjz/Whats-App-Image-2021-12-10-at-21-44-45.jpg)](https://postimg.cc/YGCGG5cH)

Dengan keterangan :

Doriki adalah DNS Server

Jipangu adalah DHCP Server

Maingate dan Jorge adalah Web Server

Jumlah Host pada Blueno adalah 100 host

Jumlah Host pada Cipher adalah 700 host

Jumlah Host pada Elena adalah 300 host

Jumlah Host pada Fukurou adalah 200 host

## B . Subnetting
Luffy ingin meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM. setelah melakukan subnetting. Disini kelompok kami menggunakan subnetting VLSM :

![](https://i.postimg.cc/VLJqvdjz/image.png)

![](https://i.postimg.cc/MGp1QD4f/image.png)

Dengan VLSM tree sebagai berikut :

![]()

## C . Routing
diharuskan melakukan Routing agar setiap perangkat pada jaringan tersebut dapat terhubung.
- Konfigurasi GNS3 & Routing
  - FOOSHA
    ```
    auto eth0
    iface eth0 inet static
      address 192.168.122.2
      netmask 255.255.255.252
      gateway 192.168.122.1
    auto eth1
    iface eth1 inet static
        address 10.0.7.145
        netmask 255.255.255.252
    auto eth2
    iface eth2 inet static
        address 10.0.7.149
        netmask 255.255.255.252
    ```
    Routing
    ```
    route add -net 10.0.7.0 netmask 255.255.255.128 gw 10.0.7.146
    route add -net 10.0.0.0 netmask 255.255.252.0 gw 10.0.7.146
    route add -net 10.0.7.128 netmask 255.255.255.248 gw 10.0.7.146
    route add -net 10.0.4.0 netmask 255.255.254.0 gw 10.0.7.150
    route add -net 10.0.6.0 netmask 255.255.255.0 gw 10.0.7.150
    route add -net 10.0.7.136 netmask 255.255.255.248 gw 10.0.7.150
    ```
    
  - WATER7
    ```
    auto eth0
    iface eth0 inet static
        address 10.0.7.146
        netmask 255.255.255.252
        gateway 10.0.7.145
    auto eth1
    iface eth1 inet static
        address 10.0.7.1
        netmask 255.255.255.128
    auto eth2
    iface eth2 inet static
        address 10.0.7.129
        netmask 255.255.255.248
    auto eth3
    iface eth3 inet static
        address 10.0.0.1
        netmask 255.255.252.0
    ```
    Routing
    ```
    route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.0.7.145
    ```
    
  - GUANHAO
    ```
    auto eth0
    iface eth0 inet static
        address 10.0.7.150
        netmask 255.255.255.252
        gateway 10.0.7.149
    auto eth1
    iface eth1 inet static
        address 10.0.4.1
        netmask 255.255.254.0
    auto eth2
    iface eth2 inet static
        address 10.0.7.137
        netmask 255.255.255.248
    auto eth3
    iface eth3 inet static
        address 10.0.6.1
        netmask 255.255.255.0
    ```
    Script
    ```
    route add -net 0.0.0.0 netmask 0.0.0.0 gw 10.0.7.149
    ```
  - DORIKI
    ```
    auto eth0
    iface eth0 inet static
        address 10.0.7.130
        netmask 255.255.255.248
        gateway 10.0.7.129
    ```
  - JIPANGU
    ```
    auto eth0
    iface eth0 inet static
        address 10.0.7.131
        netmask 255.255.255.248
        gateway 10.0.7.129
    ```
  - JORGE
    ```
    auto eth0
    iface eth0 inet static
      address 10.0.7.138
      netmask 255.255.255.248
      gateway 10.0.7.137
    ```
  - MAINGATE
    ```
    auto eth0
    iface eth0 inet static
      address 10.0.7.139
      netmask 255.255.255.248
      gateway 10.0.7.137
    ```

## D . Pemberian IP
Tugas berikutnya adalah memberikan ip pada subnet Blueno, Cipher, Fukurou, dan Elena secara dinamis menggunakan bantuan DHCP server. Kemudian kalian ingat bahwa kalian harus setting DHCP Relay pada router yang menghubungkannya.
- JIPANGU (DHCP Server)
  - Instalasi
    ```
    apt-get update
    apt-get install isc-dhcp-server -y
    service isc-dhcp-server start
    ```
  - Tambahkan interface di /etc/default/isc-dhcp-server
    ```
    INTERFACES="eth0"
    ```
  - Tambahkan konfigurasi di /etc/dhcp/dhcpd.conf
    ```
    subnet 10.0.7.128 netmask 255.255.255.248 {
      option routers 10.0.7.129;
    }
    subnet 10.0.7.0 netmask 255.255.255.128 {
      range 10.0.7.1 10.0.7.126;
      option routers 10.0.7.1;
      option broadcast-address 10.0.7.127;
      option domain-name-servers 10.0.7.130;
      default-lease-time 360;
      max-lease-time 7200;
    }
    subnet 10.0.0.0 netmask 255.255.252.0 {
      range 10.0.0.1 10.0.3.254;
      option routers 10.0.0.1;
      option broadcast-address 10.0.3.255;
      option domain-name-servers 10.0.7.130;
      default-lease-time 360;
      max-lease-time 7200;
    }
    subnet 10.0.4.0 netmask 255.255.254.0 {
            range 10.0.4.1 10.0.5.254;
            option routers 10.0.4.1;
            option broadcast-address 10.0.5.255;
            option domain-name-servers 10.0.7.130;
            default-lease-time 360;
            max-lease-time 7200;
    }
    subnet 10.0.6.0 netmask 255.255.255.0 {
            range 10.0.6.1 10.0.6.254;
            option routers 10.0.6.1;
            option broadcast-address 10.0.6.255;
            option domain-name-servers 10.0.7.130;
            default-lease-time 360;
            max-lease-time 7200;
    }
    ```
  - Restart
    ```
    service isc-dhcp-server restart
    ```
- WATER7 & GUANHAO (DHCP Relay)
  - Instalasi
    ```
    apt-get update
    apt-get install isc-dhcp-relay -y
    ```
  - Konfigurasi /etc/default/isc-dhcp-relay
    ```
    # Defaults for isc-dhcp-relay initscript
    # sourced by /etc/init.d/isc-dhcp-relay
    # installed at /etc/default/isc-dhcp-relay by the maintainer scripts
    #
    # This is a POSIX shell fragment
    #
    # What servers should the DHCP relay forward requests to?
    SERVERS="10.0.7.131"
    # On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
    INTERFACES="eth0 eth1 eth2 eth3"
    # Additional options that are passed to the DHCP relay daemon?
    OPTIONS=""
    ```
  - Restart
    ```
    service isc-dhcp-relay restart
    ```
- Konfigurasi GNS3 Client BLUENO, CIPHER, ELENA, FUKUROU
  ```
  auto eth0
  iface eth0 inet dhcp
  ```

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi Foosha menggunakan iptables, tetapi Luffy tidak ingin menggunakan MASQUERADE.
- FOOSHA
  - Konfigurasi GNS3
    ```
    auto eth0
    iface eth0 inet static
      address 192.168.122.2
      netmask 255.255.255.252
      gateway 192.168.122.1
    ```
  - Jalankan
    ```
    iptables -t nat -A POSTROUTING -s 10.0.0.0/21 -o eth0 -j SNAT --to-source 192.168.122.2
    echo nameserver 192.168.122.1 > /etc/resolv.conf
    ```
- WATER7, GUANHAO, DORIKI, JIPANGU, JORGE, MAINGATE
  ```
  echo nameserver 192.168.122.1 > /etc/resolv.conf
  ```
- DORIKI
  - Instalasi DNS
    ```
    apt-get update
    apt-get install bind9 -y
    service bind9 start
    ```
  - Tambahkan perintah berikut pada /etc/bind/named.conf.options
    ```
     forwarders {
              192.168.122.1;
      };
      allow-query{any;};
    ```
  - Komen baris
    ```
    //dnssec-validation auto;
    ```
  - Hasil
    ![image](https://i.postimg.cc/bYbvYnJx/image.png)

## Soal 2
Kalian diminta untuk mendrop semua akses HTTP dari luar Topologi kalian pada server yang merupakan DHCP Server dan DNS Server demi menjaga keamanan.
- Foosha
  - nano config.sh
  ```
  iptables -A FORWARD -p tcp --dport 80 -d 10.0.7.128/29 -i eth0 -j DROP
  ```

  ```
  # doriki DNS
  iptables -A INPUT -s 10.0.7.130 -j DROP
  # subnet a2, a3, a6, a7
  iptables -A INPUT -s 10.0.7.0/25 -j DROP
  iptables -A INPUT -s 10.0.0.0/22 -j DROP
  iptables -A INPUT -s 10.0.4.0/23 -j DROP
  iptables -A INPUT -s 10.0.6.0/24 -j DROP
  ```

  ```
  iptables -A INPUT -p tcp --dport 80 -j DROP
  ```

  Test
  ```
  nmap 10.0.7.128
  nmap 10.0.7.131
  ```
  ![image](https://i.postimg.cc/wM6xbfp9/image.png)

## Soal 3
Karena kelompok kalian maksimal terdiri dari 3 orang. Luffy meminta kalian untuk membatasi DHCP dan DNS Server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan menggunakan iptables, selebihnya didrop.

Pada DNS Server Doriki kita akan menggunakan command :

```
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```
command tersebut akan menjadikan pembatasan penerimaan koneksi ICMP menjadi maksimal 3 dan jika lebih akan di drop.

## Soal 4
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut :
Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

- DORIKI
  ```
  iptables -A INPUT -s 10.0.7.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
  iptables -A INPUT -s 192.170.7.0/25 -j REJECT
  
  iptables -A INPUT -s 10.0.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
  iptables -A INPUT -s 10.0.0.0/22 -j REJECT
  ```


## Soal 5
Kemudian kalian diminta untuk membatasi akses ke Doriki yang berasal dari subnet Blueno, Cipher, Elena dan Fukuro dengan beraturan sebagai berikut :
Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.
Selain itu di reject

- DORIKI
  ```
  iptables -A INPUT -s 10.0.4.0/23 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
  iptables -A INPUT -s 10.0.4.0/23 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
  iptables -A INPUT -s 10.0.4.0/23 -j REJECT
  
  iptables -A INPUT -s 10.0.6.0/24 -m time --timestart 15:01 --timestop 23:59 -j ACCEPT
  iptables -A INPUT -s 10.0.6.0/24 -m time --timestart 00:00 --timestop 06:59 -j ACCEPT
  iptables -A INPUT -s 10.0.6.0/24 -j REJECT
  ```
  Hal ini menyebabkan akses dari subnet Elena dan Fukurou ke DNS Server Doriki hanya bisa dilakukan setiap hari mulai pukul 15.01 sampai dengan pukul 06.59 dan selain jam tersebut akan di reject.


## Soal 6
Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate.

Pada Gunhao, dijalankan command seperti berikut :

```
iptables -A -PREROUTING -t -nat -p tcp -d 10.0.0.18 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 10.0.0.26
iptables -A -PREROUTING -t nat -p tcp -d 10.0.0.18 -j DNAT --to-destination 10.0.0.26
```
