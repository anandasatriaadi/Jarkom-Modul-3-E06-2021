# Jarkom-Modul-3-E06-2021

## Nomor 1
###	Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server

### _Solusi_
   ![image27](https://user-images.githubusercontent.com/36522826/141447617-9e2fb848-67c6-45db-a3cb-108a704162a9.png)
   Terbuatlah topologi seperti gambar diatas, kemudian install `bind9` di Enieslobby, `isc-dhcp-server` di Jipangu, dan `squid` di water 7

   ```
   Di EniesLobby
   apt-get install bind9
   
   Di Jipangu
   apt-get install isc-dhcp-server
   
   Di Water7
   apt-get install squid
   ```

## Nomor 2
###	Dan Foosha sebagai DHCP Relay. Luffy dan Zoro menyusun peta tersebut dengan hati-hati dan teliti.

### _Solusi_
   Di foosha di-install `isc-dhcp-relay` Kemudian edit `/etc/default/isc-dhcp-relay` dan isikan seperti gambar dibawah
   ![image18](https://user-images.githubusercontent.com/36522826/141447683-66730d38-94cf-4c6b-ad1d-4477884bc4f3.png)
   
   Kemudian restart `isc-dhcp-relay` pada foosha
   
## Nomor 3
###	Ada beberapa kriteria yang ingin dibuat oleh Luffy dan Zoro, yaitu: Semua client yang ada __HARUS__ menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169

### _Solusi_
   Edit semua client yang ada di topologi dari yang menggunakan static menjadi menggunakan konfigurasi DHCP bisa dengan merubahnya di `/etc/network/interfaces`
   ![image28](https://user-images.githubusercontent.com/36522826/141448703-f0e8bcb9-67dc-4bc2-8bdf-6786c95b334b.png)
   
   Kemudian agar bisa connect dengan internet setiap clientnya, ubah `/etc/bind/named.conf.options` di EniesLobby
   ![image20](https://user-images.githubusercontent.com/36522826/141448786-57589994-13ce-45f3-a898-ddb6cfa366f8.png)
   
   Kemudian restart bind9 dan setiap client sudah terhubung ke internet. Kemudian ke node Jipangu dan edit file `/etc/default/isc-dhcp-server` dan edit bagian `INTERFACES`
   ![image12](https://user-images.githubusercontent.com/36522826/141448869-b80b25b8-9a6a-4c4e-b3f0-d522d44eb309.png)
   
   Setelah itu edit file `/etc/dhcp/dhcpd.conf` dan masukkan konfigurasi seperti dibawah
   ![image7](https://user-images.githubusercontent.com/36522826/141448916-d6f91832-4150-467d-944e-266e7acf1727.png)
   
   Setelah di save, restart service `isc-dhcp-server`
   
## Nomor 4
###	Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50

### _Solusi_
   Tambahkan konfigurasi dibawah ke `/etc/dhcp/dhcpd.conf`
   ![image16](https://user-images.githubusercontent.com/36522826/141449469-d33181c4-7c66-4eac-9e7f-f735e195b34c.png)
   
   Setelah di save, restart service `isc-dhcp-server`

## Nomor 5
###	Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut. (5)

### _Solusi_
   Edit `etc/dhcp/dhcp.conf` dan mengubah `domain-name-servers`
   ![image5](https://user-images.githubusercontent.com/36522826/141449621-73eb2c02-3fb8-49e0-8141-7d93f1f8d09c.png)

   Setelah di save, restart service `isc-dhcp-server`
   
## Nomor 6 <br/>
###	Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama **6 menit** sedangkan pada client yang melalui Switch3 selama **12 menit**. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama **120 menit**. <br/>

### _Solusi_ 
Pada `Jipangu` edit `etc/dhcp/dhcpd.conf`. Pada 10.32.1.0 (Switch 1) ubah `default-lease-time = 360` dan `max-lease time = 7200`. Pada 10.32.3.0 (Switch 1) ubah `default-lease-time = 720` dan `max-lease time = 7200`. <br/>

![image](https://user-images.githubusercontent.com/57354564/141469963-532f8e93-ad87-4a53-b47d-987c492fa120.png)

## Nomor 7 <br/>
###	Luffy dan Zoro berencana menjadikan `Skypie` sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69 <br/>

### _Solusi:_ 
Pada `Skypie` configure `/etc/network/interfaces` dengan **fixed hardware address** sbg berikut: <br/>

![image](https://user-images.githubusercontent.com/57354564/141470111-e6a6c6fa-6e4f-4753-ac4d-f228162c6442.png)

Kemudian pada `Jipangu` edit `/etc/dhcp/dhcpd.conf` dan tambahkan seperti gambar berikut untuk memberikan **Skypie fixed address**

![image](https://user-images.githubusercontent.com/57354564/141470228-e51377bb-d68d-4dd8-a060-50f245d66440.png)

## Nomor 8
###	Loguetown digunakan sebagai client Proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi. Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

### _Solusi_ 
Pertama pada EniesLobby atur dns supaya jualbelikapal.e06.com mengarah pada Water7 (10.32.2.3)

![image](https://user-images.githubusercontent.com/57354564/141470431-4d0c1f1e-e469-4f99-8cb8-cd202d97cda4.png)
![image](https://user-images.githubusercontent.com/57354564/141470494-4073d9f2-79c8-4a8b-90ac-94829042144e.png)

Ubah nameserver pada Loguetown menjadi IP dari eniesLobby.

Atur squid pada Water7 sebagai proxy server. Edit /etc/squid/squid.conf menjadi sebagai berikut:

![image](https://user-images.githubusercontent.com/57354564/141470607-ce54256b-5291-4eae-ae95-30cc1ae9953c.png)

Loguetown sebagai client proxy diatur sebagai berikut:

![image](https://user-images.githubusercontent.com/57354564/141470808-0216c7d9-2c51-4107-84e2-cd6635d83591.png)

Kita coba melakukan lynx ke its.ac.id maka hasil sbg berikut:

![image](https://user-images.githubusercontent.com/57354564/141470866-a42b56d6-9299-4a60-9ffc-bf3904565ac2.png)

## Nomor 9
###	Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy
### _Solusi_ 
Menambahkan passwd di /etc/squid/passwd
![image](https://user-images.githubusercontent.com/57354564/141470898-43a96c23-40f1-45c8-a201-debefdff5c2e.png)

Kemudian edit /etc/squid/squid.conf pada Water7 dan tambahkan baris berikut
![image](https://user-images.githubusercontent.com/57354564/141470915-298c6744-111f-443c-b029-06e8f32634b6.png)

Setelah mencoba melakukan lynx pada Loguetown akan diminta password
![image](https://user-images.githubusercontent.com/57354564/141470940-6b421256-1b85-4ce8-8987-8334d0196611.png)

## Nomor 10
### Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

### _Solusi_
Di Water7
Buat `/etc/squid/acl.conf`

![image](https://user-images.githubusercontent.com/43901559/141468235-27267b66-efda-432a-a3ce-1092b0e9fdfd.png)

Tambahkan di `/etc/squid/squid.cond`
```
include /etc/squid/acl.conf

http_access deny DENY_1
http_access deny DENY_2
http_access deny DENY_3
http_access deny DENY_4
http_access deny DENY_5
http_access deny DENY_6
http_access deny DENY_7

http_access deny all
```

Coba di waktu yang dibatasi

![image](https://user-images.githubusercontent.com/43901559/141468266-3892e4e5-dcd2-4316-b72d-f53c3fb9fb54.png)

![image](https://user-images.githubusercontent.com/43901559/141468279-fb6ac9f2-6c15-48fb-b6b6-bf8e1643ccc2.png)

![image](https://user-images.githubusercontent.com/43901559/141468302-bb07ee1d-252b-4114-898c-3bde685fee13.png)

Coba di waktu yang diperbolehkan akses

![image](https://user-images.githubusercontent.com/43901559/141468327-2a761e89-6a55-424a-aa37-4971e8d5fda5.png)

![image](https://user-images.githubusercontent.com/43901559/141468340-98422d89-2620-4d54-a4e2-f600a6e84e4f.png)


## Nomor 11
### Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

### _Solusi_
Di EniesLobby
Tambahkan di `/etc/bind/named.conf.local`
```
zone "super.franky.e06.com" {
        type master;
        file "/etc/bind/kaizoku/super.franky.e06.com";
        allow-transfer { 10.32.3.69; };
};
```
Buat folder `/etc/bind/kaizoku/`

Buat file `super.franky.e06.com` di dalamnya

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     super.franky.e06.com.   root.super.franky.e06.com. (
                     2021110801         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      super.franky.e06.com.
@       IN      A       10.32.3.69
www     IN      CNAME   super.franky.e06.com.
```

Di Skypie

Install apache2 `apt-get install apache2`

Di `/etc/apache2/apache2.conf`, tambahkan

```
ServerName 10.32.3.69
```

Buat `super.franky.e06.com.conf` di `/etc/apache2/sites.available`

Lalu tambahkan
```
ServerName super.franky.e06.com
ServerAlias www.super.franky.e06.com

ServerAdmin webmaster@localhost
DocumentRoot /var/www/super.franky.e06.com

<Directory /var/www/super.franky.e06.com>
  Options +Indexes
  AllowOverride All
</Directory>
```

Di Water7

Tambahkan di `/etc/squid/squid.conf`

```
acl BLACKLIST dstdomain google.com
deny_info http://super.franky.e06.com/ BLACKLIST
http_reply_access deny BLACKLIST
```

Ubah nameserver jadi `10.32.2.2`

Coba `lynx google.com` di LogueTown

![image](https://user-images.githubusercontent.com/43901559/141468823-15029ad0-c265-454d-9eca-cf77d9fd1ab9.png)

![image](https://user-images.githubusercontent.com/43901559/141468832-7f91abc8-9691-41dc-b0bd-8565f636e699.png)


## Nomor 12 & 13
### Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps (12). Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya (13).

### _Solusi_

Zoro mencoba mendownload gambar .png dan .jpg
![image](https://user-images.githubusercontent.com/43901559/141468959-11b8de6f-8f64-498c-a6e0-ac11d2f2069d.png)
