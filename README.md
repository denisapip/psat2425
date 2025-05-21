# Penilaian Sumatif Akhir Tahun
## Mapil DevOps XI TJKT 1 - Penilaian Praktek
### SMKN 1 Banyumas - TA. 2024 2025

#
# Cara mendeploy Aplikasi

## 1. Buat File .env

File .env adalah file environment sistem mirip seperti file konfig.php
#
isi file .env sebagai berikut

```.env
DB_USER=....  (isi dengan user RDS)
DB_PASS=....  (isi dengan password RDS)
DB_NAME=....  (isi dengan nama database yang akan dibuat di RDS)
DB_HOST=....  (isi dengan Endpoint RDS)
```

contoh:

```.env
DB_USER=admin
DB_PASS=P4ssw0rd123
DB_NAME=psat2425
DB_HOST=rdsku.czt6n8ylfvyb.us-east-1.rds.amazonaws.com
```

## 2. Jalankan 
Jalankan dengan username dan password default berikut ini
#
### username = admin
### password = 123
#

Kemudian inputkanlah data sesuai dengan datamu

# Panduan Deployment psat2425

## Gambaran Umum
Panduan lengkap untuk deploy aplikasi PSAT2425 di AWS menggunakan UserData dengan konfigurasi EC2 dan RDS yang tepat.

## Persyaratan
1. Akun AWS dengan akses EC2 dan RDS
2. Akun GitHub
   
## Langkah 1: Persiapan Awal

1. *Unduh File dari GitHub*  
    https://github.com/paknux/psat2425
   

2. *Buat Repositori Baru*  
   - Buat repositori baru dengan nama psat2425 di GitHub Anda
   - Push semua file ke repositori baru Anda

## Langkah 2: Konfigurasi Security Groups

1. Buat security group SG-ServerDB
   allow inbound  MySQL (3306) from anywhereIPv4 (0.0.0.0/0)  

2. Buat security group SG-ServerWeb
   allow inbound SSH (22) from anywhereIPv4 (0.0.0.0/0)  
   allow inbound HTTP (80) from anywhereIPv4 (0.0.0.0/0)  
   allow inbound HTTPS (443) from anywhereIPv4 (0.0.0.0/0)  

## Langkah 3: Setup RDS Database

1. Buat RDS MySQL instance:
   - Metode Pembuatan: Standard create
   - Engine: MySQL
   - Template: Free tier
   - DB cluster identifier: biankards(bebas)
   - Master username: admin
   - Master password: P4ssw0rd123(bebas)
   - Public access: no
   - VPC security group: SG-ServerDB

2. Setelah RDS ready, dapatkan endpoint database, bisa dicek dengan MySQL client (MySQLFront, HeidiSQL)
3. Buat Database dan tabel jika diperlukan dengan MySQL Client kita
   
## Langkah 4: Deployment EC2 dengan UserData

1. Buka AWS Console > EC2 > Launch Instance
2. Name and tags: psatb(bebas)
3. AMI: Ubuntu
4. Pilih instance type t2.nano
5. Pilih security group SG-ServerWeb yang sudah dibuat
6. Di bagian "Configure Instance":
   - Scroll ke bawah sampai menemukan bagian "Advanced Details"
   - Klik tombol kuning "Advanced Details" untuk memperluas
   - Di bagian "User data", pilih "As text"
   - Masukkan script berikut:

```.env
#!/bin/bash
apt update -y
apt install -y apache2 php php-mysql libapache2-mod-php mysql-client
rm -rf /var/www/html/{*,.*}
git clone https://github.com/denisapip/psat2425 /var/www/html
chmod -R 777 /var/www/html
echo DB_USER=admin > /var/www/html/.env
echo DB_PASS=P4ssw0rd123  >> /var/www/html/.env
echo DB_NAME=crudsiswa  >> /var/www/html/.env
echo DB_HOST=biankards.cco8dazr6fvq.us-east-1.rds.amazonaws.com >> /var/www/html/.env
apt install openssl
a2enmod ssl
a2ensite default-ssl.conf
systemctl reload apache2
```

7. Setelah itu Launch instance

## Verifikasi Deployment

Setelah menyelesaikan semua langkah di atas, ikuti petunjuk berikut untuk memverifikasi bahwa aplikasi berhasil terdeploy:

1. **Dapatkan Public IP Instance**:
   - Buka AWS EC2 Console
   - Pilih instance yang baru dibuat
   - Salin alamat **IPv4 Public IP** dari bagian detail instance

2. **Akses Aplikasi**:
   - Buka browser web
   - Masukkan alamat berikut di address bar:
     http://<public-ip-anda>
   - Ganti `<public-ip-anda>` dengan IP yang disalin sebelumnya

3. **Hasil**:
   - Setelah berhasil akan muncul tampilan aplikasi psat2425
   - Jalankan dengan username dan password default berikut ini
#
### username = admin
### password = 123
#
   - Kemudian inputkanlah data sesuai dengan datamu
     
#
# Pengumpulan Hasil
Catat Link repositry anda

Screenshoot halaman Data Siswa (dashboard.php) yang sudah ada namamu

Kumpulkan ke Form yang ada di dalam GC 

#
