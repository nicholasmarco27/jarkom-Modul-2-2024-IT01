![Screenshot 2024-05-03 204532](https://github.com/nicholasmarco27/jarkom-Modul-2-2024-IT01/assets/80316798/19c19f25-64dd-419d-bdd2-7f967ee57b13)# Laporan Resmi Jarkom Modul 2
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


