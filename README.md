# Jarkom-Modul-2-B01-2021

<li> Jonathan Timothy Siregar - 05111940000120
<li> Muhammad Fikri Sandi Pratama - 05111940000195
<li> Mohammad Thoriq Huda - 05111940000207
<br><br><br>

  ## 1. Topologi
  Soal 1 Membuat topologi dengan kriteria EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2   Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.

  ![1_1](https://user-images.githubusercontent.com/55092974/139522327-9ea6e439-e665-49dc-b500-b1fb4ee1ca93.JPG)
  
  - Foosha (dijakan sebagai router) <br>
  Atur konfigurasi seperti berikut :
  ```bash
  auto eth0
  iface eth0 inet dhcp
 
  auto eth1
  iface eth1 inet static
	address 192.177.1.1
	netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
	address 192.177.2.1
	netmask 255.255.255.0
  ```
  - loguetown (akan dijadikan client) <br>
  Atur konfigurasi seperti berikut :
  ```bash
  auto eth0
  iface eth0 inet static
	address 192.177.1.2
	netmask 255.255.255.0
	gateway 192.177.1.1
  ```

  - Alabasta (akan dijadikan client) <br>
  Atur konfigurasi seperti berikut :
  ```bash
  auto eth0
  iface eth0 inet static
	address 192.177.1.3
	netmask 255.255.255.0
	gateway 192.177.1.1
  ```
  - EniesLobby (akan menjadi DNS Master) <br>
  Atur konfigurasi seperti berikut :
  ```bash
  auto eth0
  iface eth0 inet static
	address 192.177.2.2
	netmask 255.255.255.0
	gateway 192.177.2.1
  ```
  - Water7 (akan menjadi DNS Slave) <br>
  Atur konfigurasi seperti berikut :
  ```bash
  auto eth0
  iface eth0 inet static
	address 192.177.2.3
	netmask 255.255.255.0
	gateway 192.177.2.1
  ```
  - Skypie (akan menjadi Web Server) <br>
  Atur konfigurasi seperti berikut :
  ```bash
  auto eth0
  iface eth0 inet static
	address 192.177.2.4
	netmask 255.255.255.0
	gateway 192.177.2.1
  ```
Setelah mengatur konfigurasi dari foosha dan Node yang lain, selanjutnya yaitu
melakukan pengisian script bash di `nano .bashrc` yang bertujuan 
agar script program bash bisa dijalankan otomatis ketika node di jalankan.

 - nano .bashrc di Foosha <br>
hanya berisi `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.177.0.0/16`

  - nano .basrc di node yang lain (client)
  jika manual menambahkan `nameserver 192.168.122.1 > /etc/resolv.conf` kepada node ubuntu yang lain. <br>
agar otomatis bisa dimasukkan kedalam .bashrc yang isinya `echo 'nameserver 192.168.122.1' > /etc/resolv.conf // IP Foosha`

Kemudian untuk mengecek apakah sudah dapat terkoneksi ke internet, ketikkan ping google.com pada salah satu node ubuntu. Disini menggunakan node Loguetown.
  
  ![1 2](https://user-images.githubusercontent.com/55092974/139522333-0d0728b4-f45e-4e59-81a0-378543812210.JPG)
  
  ## 2.  Pembuatan Website Utama
soal 2 Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses `franky.b01.com` dengan alias `www.franky.b01.com` pada folder kaizoku. <br>

- Pertama lakukan perintah  `apt-get update` dan `apt-get install bind9 -y` pada `EniesLobby` <br>

- Kemudian lakukan `nano /etc/bind/named.conf.local` <br>

- Kemudian Edit root dan localhost menjadi franky.b01.com (b01 nomer kelompok)

```bash
echo 'zone "franky.b01.com" {
        type master;
        file "/etc/bind/kaizoku/franky.b01.com";
};
```

- kemudian membuat folder kaizoku dengan cara <br>

`mkdir /etc/bind/kaizoku` <br>

- lalu Copykan file db.local pada path `/etc/bind` ke dalam folder kaizoku yang baru saja dibuat dan ubah namanya menjadi `franky.b01.com` seperti dibawah ini : <br>

`cp /etc/bind/db.local /etc/bind/kaizoku/franky.b01.com` <br>

- Kemudian buka file franky.b01.com dan edit seperti gambar berikut dengan IP EniesLobby. `nano /etc/bind/kaizoku/franky.b01.com`
```bash
;BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.B01.com. root.franky.B01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      franky.b01.com.
@               IN      A       192.177.2.2 ;
www             IN      CNAME   franky.b01.com.
```
- lalu `service bind9 restart`

- kemudian cek ping `franky.b01.com` atau `www.franky.b01.com` disini menggunankan client `Loguetown` seperti gambar berikut, dan IPnya mengarah ke EniesLobby <br>

![2 1](https://user-images.githubusercontent.com/55092974/139525417-bccfed28-ad35-4648-be0e-6906cf5e2112.JPG)
	
	
  ## 3. Pembuatan Subdomain
soal 3 membuat subdomain `super.franky.b01.com` dengan alias `www.super.franky.b01.com` yang diatur DNS nya di EniesLobby dan mengarah ke Skypie <br>
	
- Melanjutkan nomer 2 dengan cara edit `nano /etc/bind/kaizoku/franky.b01.com` <br>

```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.B01.com. root.franky.B01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      franky.b01.com.
@               IN      A       192.177.2.2 ;
#Menambahkan dibawah ini
www             IN      CNAME   franky.b01.com.
super		IN	A	192.177.2.3 ; IP Skypie
```

- Lalu Restart service bind dengan cara `service bind9 restart`

- kemudian cek ping `super.franky.b01.com` atau `www.super.franky.b01.com` di client `Loguetown` seperti gambar berikut : <br>

![3 1](https://user-images.githubusercontent.com/55092974/139526226-39e7403c-b307-44cc-a08e-1b6672c79fc7.JPG)
	
	
  ## 4. Pembuatan Reverse Domain

  
  Soal 4 meminta untuk membuat sebuah Reverse Domain
  
  ## 5. Pembuatan DNS Slave
  
  Soal 5 meminta untuk membuat suatu Reverse Domain pada Water7 yang merupakan DNS Slave, supaya tetap bisa menghubungi Franky jika server EniesLobby rusak. 
  
  ## 6. Pendelegasian Subdomain

Terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.
	
Di EniesLobby:

 mengedit file `/etc/bind/kaizoku/franky.b01.com` dengan menambahkan:

```bash
        ns2     IN      A       192.177.2.3 ; IP Water7
        mecha   IN      NS      ns2
```
edit file `/etc/bind/named.conf.options` dengan comment bagian `dnssec-validation auto;` dan menambahkan line `allow-query{any;};`. 
	
	
Di Water7:

 edit file `/etc/bind/named.conf.options`, comment bagian `dnssec-validation auto;` tambah line `allow-query{any;};
Kemudian tambah zone pada `/etc/bind/named.conf.local` dengan menambahkan:

```bash
    zone "mecha.franky.b01.com" {
            type master;
            file "/etc/bind/sunnygo/mecha.franky.b01.com";
    };
```
buat folder sunnygo di `/etc/bind/db.local` menjadi `/etc/bind/sunnygo/mecha.franky.b01.com`. ganti SOA menjadi `mecha.franky.b01.com.`, NS `mecha.franky.b01.com.`, record A yang mengarah ke `IP Skypie`, dan CNAME `www` pada `mecha.franky.b01.com.	

  ## 7. Subdomain dari Water7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Franky dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.<br>
	
Di Water7:

Mengedit file `/etc/bind/sunnygo/mecha.franky.b01.com` dengan menambahkan:

```bash
        ns1     IN      A       192.177.2.4 ; IP Skypie
        general IN      NS      ns1
```
menambahkan zone pada `/etc/bind/named.conf.local` dengan menambahkan:

```bash
    zone "general.mecha.franky.b01.com" {
            type master;
            file "/etc/bind/sunnygo/general.mecha.franky.b01.com";
    };	
```	
copy `/etc/bind/db.local` menjadi `/etc/bind/sunnygo/general.mecha.franky.b01.com`. ganti SOA menjadi `general.mecha.franky.b01.com.`, NS `general.mecha.franky.b01.com.`, record A yang mengarah ke `IP Skypie`, dan CNAME `www` pada `general.mecha.franky.b01.com.`.
  
  ## 8. Konfigurasi Webserver
 Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver `www.franky.b01.com`. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada `/var/www/franky.b01.com`.
### EniesLobby
- Pertama lakukan edit file `/etc/bind/kaizoku/franky.b01.com` mengganti IPnya dari `EniesLobby` ke `Skypie` seperti pada gambar berikut:
```bash
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.B01.com. root.franky.B01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      franky.b01.com.
# ganti IP ke Skypie
@               IN      A       192.177.2.4 ;
www             IN      CNAME   franky.b01.com.
; Nomer 3 Subdomain dan Alias mengarah ke skypie
super           IN      A       192.177.2.4 ; Ip skypie
www.super       IN      CNAME   super.franky.b01.com.
ns2             IN      A       192.177.2.3;	
```
- lalu `jalankan service bind9 restart`

### Skypie
- lakukan install aplikasi yang diperlukan yaitu `apache, PHP, dan libapache2-mod-php7.0.` pada `skypie`

```bash
apt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y
```
- Kemudian menambahkan file configurasi pada folder `/etc/apache2/sites-available` dengan nama `franky.b01.com.conf`

```bash
<VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/franky.b01.com
        ServerName franky.b01.com
        ServerAlias www.franky.b01.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```
- Lalu download file requirement menggunakan `wget`.

```bash
wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/raw/main/franky.zip
```
- Lalu membuat folder `franky.b01.com` dalam folder `/var/www/` 
```bash
mkdir /var/www/franky.b01.com
```
- Kemudian unzip file requirement dan disimpan dalam folder `/var/www/franky.b01.com`
```bash
unzip -j  franky.zip -d /var/www/franky.b01.com
```
![8 1](https://user-images.githubusercontent.com/55092974/139527155-a35f0cf8-af9b-4fcd-b6e8-cd1f71baa542.JPG)

- lalu aktifkan `franky.b01.com` dengan command
```bash
cd /etc/apache2/sites-available/
a2ensite franky.b01.com.conf
cd
```
- Restart apache `service apache2 restart`

### Logouetown

- Pada `loguetown` install lynx `apt-get install lynx -y`
- Buka `www.franky.b01.com` menggunakan lynx di client Loguetown atau Alabasta.

![8 2](https://user-images.githubusercontent.com/55092974/139527216-838c00ef-5319-4d2c-a5b3-ccb644b2c0e4.JPG)
	
  ## 9. Pengubahan URL
  
  ## 10. Penyimpanan Aset pada Subdomain
  
  ## 11. Directory Listing
  
  ## 12. Persiapan Error File
  
  ## 13. Pembuatan Konfigurasi Virtual Host
  
  ## 14. Pembatasan Akses Port
  
  ## 15. Autentikasi
  
  ## 16. Pengalihan Akses IP
  
  ## 17. Penggantian Request Gambar
  
  
  ### Kendala yang dialami
  
