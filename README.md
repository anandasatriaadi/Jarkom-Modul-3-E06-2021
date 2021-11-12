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
