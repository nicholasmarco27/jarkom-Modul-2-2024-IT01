# Laporan Resmi Jarkom Modul 2
## Kelompok IT01
| NRP | Nama |
| ------ | ------ |
| 5027221042 |Nicholas Marco Weinandra |
| 5027221052 | Mochamad Zidan Hadipratama |

# Soal 1
Untuk membantu pertempuran di Erangel, kamu ditugaskan untuk membuat jaringan komputer yang akan digunakan sebagai alat komunikasi. Sesuaikan rancangan Topologi dengan rancangan dan pembagian yang berada di link yang telah disediakan, dengan ketentuan nodenya sebagai berikut :
- DNS Master akan diberi nama Pochinki, sesuai dengan kota tempat dibuatnya server tersebut
- Karena ada kemungkinan musuh akan mencoba menyerang Server Utama, maka buatlah DNS Slave Georgopol yang mengarah ke Pochinki
- Markas pusat juga meminta dibuatkan tiga Web Server yaitu Severny, Stalber, dan Lipovka. Sedangkan Mylta akan bertindak sebagai Load Balancer untuk server-server tersebut

## Setup
- Jalankan command berikut pada terminal Erangel agar node dapat terhubung ke internet.
```
# Masquerade
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.64.0.0/16
```

- Ubah nameserver menggunakan nameserver IP Erangel
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
cat /etc/resolv.conf
```

- Install Bind9 pada node Pochinki
```
apt-get update
apt-get install bind9 -y
```

# Soal 2
Karena para pasukan membutuhkan koordinasi untuk mengambil airdrop, maka buatlah sebuah domain yang mengarah ke Stalber dengan alamat airdrop.xxxx.com dengan alias www.airdrop.xxxx.com dimana xxxx merupakan kode kelompok. Contoh : airdrop.it01.com

- Edit file `/etc/bind/named.conf.local`
```
nano /etc/bind/named.conf.local
```
![Screenshot 2024-05-03 203213](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/ab0e1ec2-65b0-4316-ac0b-0c69fd6226c2)

- Buat folder baru untuk domain airdrop.it01.com
```
mkdir /etc/bind/airdrop
```

- Copy file db.local ke file konfigurasi domain
```
cp /etc/bind/db.local /etc/bind/airdrop/airdrop.it01.com
```

- Edit file /etc/bind/airdrop/airdrop.it01.com
![Screenshot 2024-05-03 203611](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/112256bc-e208-4ab2-8242-ef211eb1c6d1)

- Restart bind9
```
service bind9 restart
```

## Testing
- ping airdrop.it01.com dari node Severny
![Screenshot 2024-05-03 203744](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/3b0dacec-3386-419c-a999-54f890b86583)

- ping www.airdrop.it01.com
![Screenshot 2024-05-03 203817](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/9fed93a9-5a4e-4b9c-834c-9b2e51d834bd)
airdrop.it01.com dari node Severny


# Soal 3
Para pasukan juga perlu mengetahui mana titik yang sedang di bombardir artileri, sehingga dibutuhkan domain lain yaitu redzone.xxxx.com dengan alias www.redzone.xxxx.com yang mengarah ke Severny

- Edit file `/etc/bind/named.conf.local`
```
nano /etc/bind/named.conf.local
```
![Screenshot 2024-05-03 204000](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/44885120-56f2-4074-a48f-80573ab4ee60)

- Buat folder baru untuk domain redzone.it01.com
```
mkdir /etc/bind/redzone
```
- Copy file db.local ke file konfigurasi domain
```
cp /etc/bind/db.local /etc/bind/redzone/redzone.it01.com
```
- Edit file /etc/bind/redzone/redzone.it01.com
![Screenshot 2024-05-03 204208](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/f485fa31-d4f7-49c6-bacb-f053231ca590)


- Restart bind9
```
service bind9 restart
```

## Testing
- ping www.redzone.it01.com dari node Stalber
![Screenshot 2024-05-03 204303](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/e848b27e-c436-4a01-8bea-cc1cc53deeee)

- ping redzone.it01.com dari node Stalber
![Screenshot 2024-05-03 204325](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/7689d00a-1497-4f1e-92c9-dd2a0da019da)

# Soal 4
Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi persenjataan dan suplai tersebut mengarah ke Mylta dan domain yang ingin digunakan adalah loot.xxxx.com dengan alias www.loot.xxxx.com

- Edit file `/etc/bind/named.conf.local`
```
nano /etc/bind/named.conf.local
```
![Screenshot 2024-05-03 204438](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/43c1ebe3-e76c-4b39-838d-7911cfec40bb)

- Buat folder baru untuk domain redzone.it01.com
```
mkdir /etc/bind/loot
```
- Copy file db.local ke file konfigurasi domain
```
cp /etc/bind/db.local /etc/bind/loot/loot.it01.com
```

- Edit file /etc/bind/redzone/redzone.it01.com
![Screenshot 2024-05-03 204532](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/829fd830-292b-43fc-86d3-64d5fdd8ed4d)


- Restart bind9
```
service bind9 restart
```

## Testing
- ping loot.it01.com dari node Stalber
![Screenshot 2024-05-03 204557](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/ab81f5a2-3007-46db-852a-09d72d00b2cb)

- ping www.loot.it01.com
![Screenshot 2024-05-03 204616](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/8ed2c387-5c7e-42bd-8033-d05e9234246f)

# Nomor 5
Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Erangel

- Quarry
![Screenshot 2024-05-03 204824](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/1e27986a-0628-4d65-8504-9a1590cae0cc)

- Shelter
![Screenshot 2024-05-03 204838](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/bfd9115f-9409-44ea-a584-0aee7c6a1f58)


- Georgopol
![Screenshot 2024-05-03 204814](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/bd16f729-66f2-42b7-b75b-daeee5cce565)

- Gatka
![Screenshot 2024-05-03 204631](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/a4c913bb-56fd-4e7c-9bf8-5cc0cc4cfe29)

- Severny
![Screenshot 2024-05-03 203817](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/de034e52-959f-479e-83b7-862b8350607f)

- Stalber
![Screenshot 2024-05-03 204616](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/cfa3686a-0f25-4ef0-9fb6-86bdb7a2e67d)

- Lipovka</br>
![Screenshot 2024-05-03 204956](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/758c4b37-063c-4eca-8d15-989d96e23b71)

- Mylta</br>
![Screenshot 2024-05-03 205122](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/290f436b-6640-4c28-8979-0e2d27f937c4)

# Soal 6
Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain redzone.xxxx.com melalui alamat IP Severny (Notes : menggunakan pointer record)

- Edit file /etc/bind/named.conf.local pada node Pochinki
```
nano /etc/bind/named.conf.local
```
Untuk reverse DNS, gunakan 3 digit pertama dari IP Severny yang dibalik, karena IP Severny kami adalah `10.64.4.2`, maka reverse-nya adalah `4.64.10`
```
zone "4.64.10.in-addr.arpa" {
    type master;
    file "/etc/bind/reverse/4.64.10.in-addr.arpa";
};
```

- Buat folder baru di /etc/bind bernama reverse
```
mkdir /etc/bind/reverse
```

- Copykan file db.local pada path /etc/bind ke dalam folder reverse yang baru saja dibuat dan ubah namanya menjadi 4.64.10.in-addr.arpa
```
cp /etc/bind/db.local /etc/bind/reverse/4.64.10.in-addr.arpa
```

- Edit file 4.64.10.in-addr.arpa menjadi seperti gambar di bawah ini
```
nano /etc/bind/reverse/4.64.10.in-addr.arpa
```
![Screenshot 2024-05-03 221751](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/a4b8c880-f333-42ad-a0c1-a6605daf9dc2)

- Restart bind9
```
service bind9 restart
```

## Testing
- Kami menggunakan node Gatka untuk Testing, pertama jalankan terlebih dahulu command berikut, pastikan untuk mengganti nameserver agar tersambung ke Erangel
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install dnsutils
```
- Kembalikan nameserver ke IP Pochinki, kemudian jalankan command berikut
```
echo nameserver 10.64.1.2 > /etc/resolv.conf
host -t PTR 10.64.4.2
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/3eb5101b-8c7e-416f-9043-dc9ea0e40240)


# Soal 7
Akhir-akhir ini seringkali terjadi serangan siber ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Georgopol untuk semua domain yang sudah dibuat sebelumnya

- Lakukan konfigurasi pada file /etc/bind/named.conf.local sebagai berikut untuk melakukan konfigurasi DNS Slave yang mengarah ke Georgopol:
```
zone "airdrop.it01.com" {
    type master;
    also-notify { 10.64.3.2; };
    allow-transfer { 10.64.3.2; };
    file "/etc/bind/airdrop/airdrop.it01.com";
};

zone "redzone.it01.com" {
    type master;
    also-notify { 10.64.3.2; };
    allow-transfer { 10.64.3.2; };
    file "/etc/bind/redzone/redzone.it01.com";
};

zone "loot.it01.com" {
    type master;
    also-notify { 10.64.3.2; };
    allow-transfer { 10.64.3.2; };
    file "/etc/bind/loot/loot.it01.com";
};

zone "4.64.10.in-addr.arpa" {
    type master;
    file "/etc/bind/reverse/4.64.10.in-addr.arpa";
};
```

- Restart Bind9
```
service bind9 restart
```

- Pada node Georgopol, lakukan `apt-get update` dan menginstall bind9 dengan `apt-get install bind9 -y`
- Lakukan konfigurasi pada file `/etc/bind/named.conf.local`
```
zone "airdrop.it01.com" {
    type slave;
    masters { 10.64.1.2; };
    file "/var/lib/bind/airdrop.it01.com";
};

zone "redzone.it01.com" {
    type slave;
    masters { 10.64.1.2; };
    file "/var/lib/bind/redzone.it01.com";
};

zone "loot.it01.com" {
    type slave;
    masters { 10.64.1.2; };
    file "/var/lib/bind/loot.it01.com";
};
```

- Restart Bind9 `service bind9 restart`

## Testing
- Stop service Bind9 pada server Pochinki dengan `service bind9 stop`
- Melakukan ping dengan server Gatka, pastikan bahwa nameserver di Gatka di set menuju IP Pochinki dan Gatka
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/7b17d47d-133c-4673-a7c4-ed38672fe93e)
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/5fca4a4a-a1c2-4271-8c4e-01a10121014a)

# Soal 8
Kamu juga diperintahkan untuk membuat subdomain khusus melacak airdrop berisi peralatan medis dengan subdomain medkit.airdrop.xxxx.com yang mengarah ke Lipovka

- Edit file `/etc/bind/airdrop/airdrop.it01.com` seperti pada gambar dibawah ini
```
nano /etc/bind/airdrop/airdrop.it01.com
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/f1d4f2ff-b415-40a4-ac74-b8d8972032e8)

- Restart Bind9 `service bind9 restart`

## Testing
- Ping `medkit.airdrop.it01.com` dari server Gatka
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/a200c64d-8a19-4ae0-ac7e-ae7e32eeed7b)

# Soal 9
Terkadang red zone yang pada umumnya di bombardir artileri akan dijatuhi bom oleh pesawat tempur. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan air raid dan memasukkannya ke domain siren.redzone.xxxx.com dalam folder siren dan pastikan dapat diakses secara mudah dengan menambahkan alias www.siren.redzone.xxxx.com dan mendelegasikan subdomain tersebut ke Georgopol dengan alamat IP menuju radar di Severny

- Pada Pochinki, buat folder baru siren
```
mkdir /etc/bind/siren
```

- Copy file dari folder redzone ke folder siren
```
cp /etc/bind/redzone/redzone.it01.com /etc/bind/siren/siren.redzone.it01.com
```

- Edit file sebagai berikut
```
nano /etc/bind/siren/siren.redzone.it01.com
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/cbbc09e1-108d-4cde-90bd-1ffb58ab5d4d)


- Edit file `/etc/bind/named.conf.options` dan tambahkan line berikut
```
allow-query{any;};
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/50014127-f0a2-4731-8b3d-d9fa3e1e81ed)

- Restart bind9 `service bind9 restart`
- Pada Georgopol, edit file `/etc/bind/named.conf.options`, kemudian tambahkan line ini
```
allow-query{any;};
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/3c8f7676-1ee1-43a8-84e2-cb7044ae237d)
- Edit file `/etc/bind/named.conf.local` kemudian tambahkan konfigurasi dibawah ini
```
zone "siren.redzone.it01.com" {
    type master;
    file "/etc/bind/siren/siren.redzone.it01.com";
};
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/15a451b1-3276-4476-aa74-f81de292cb6f)
- Buat folder siren
```
mkdir /etc/bind/siren
```
- Copy file db.local ke `/etc/bind/siren/siren.redzone.it01.com`
```
cp /etc/bind/db.local /etc/bind/siren/siren.redzone.it01.com
```
- Edit file `/etc/bind/siren/siren.redzone.it01.com`
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     siren.redzone.it01.com. root.siren.redzone.it01.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      siren.redzone.it01.com.
@       IN      A       10.64.3.2 ; IP Georgopol
www     IN      CNAME   siren.redzone.it01.com.
@       IN      AAAA    ::1
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/c6dcbe09-c7b6-4504-b5ee-c9682546036c)
- Restart Bind9 `service bind9 restart`

## Testing
- Sebelum Testing, pastikan nameserver di set ke arah Georgopol
- Ping www.siren.redzone.it01.com dari server Gatka
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/d4a0f766-0cd5-453a-b5b8-0b7438c5a7f6)
- Ping dari server Quarry
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/40f64f48-4b5c-4725-b629-ce6a12c3b7f6)
- Ping dari server Shelter
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/c4a88b11-93c9-40f0-80e8-16aeac7fdef7)

# Soal 10
Markas juga meminta catatan kapan saja pesawat tempur tersebut menjatuhkan bom, maka buatlah subdomain baru di subdomain siren yaitu log.siren.redzone.xxxx.com serta aliasnya www.log.siren.redzone.xxxx.com yang juga mengarah ke Severny

- Pada Server Georgopol buka /etc/bind/siren/siren.redzone.it01.com, kemudian edit konfigurasi seperti gambar dibawah
```
nano /etc/bind/siren/siren.redzone.it01.com
```
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/daf4c885-800d-4859-aa8f-30563005be81)

- Restart Bind9 `service bind9 restart`

## Testing
- Sebelum Testing, pastikan nameserver di set ke arah Georgopol
- ping log.siren.redzone.it01.com dari server Gatka
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/3864edb6-0de1-455a-8b88-e2baab5b5db8)
- ping www.log.siren.redzone.it01.com dari server Quarry
![image](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/350867ec-569d-43bd-9998-db6da7e4989b)


