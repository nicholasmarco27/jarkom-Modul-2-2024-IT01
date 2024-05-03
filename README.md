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


