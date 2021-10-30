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
  
  ## 3. Pembuatan Subdomain
  
  ## 4. Pembuatan Reverse Domain
  
  Soal 4 meminta untuk membuat sebuah Reverse Domain
  
  ## 5. Pembuatan DNS Slave
  
  Soal 5 meminta untuk membuat suatu Reverse Domain pada Water7 yang merupakan DNS Slave, supaya tetap bisa menghubungi Franky jika server EniesLobby rusak. 
  
  ## 6. Pendelegasian Subdomain
  
  ## 7. Subdomain dari Water7
  
  ## 8. Konfigurasi Webserver
  
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
  
