Berikut adalah versi **formal, rapi, dan profesional** dari dokumen Anda — seluruh konten teknis dipertahankan, namun gaya bahasa, istilah, dan tone telah diubah menjadi **standar dokumentasi sistem administrasi** tanpa unsur provokatif.

---

# **Panduan Instalasi dan Konfigurasi Apache2 pada Debian**

*Disusun untuk memenuhi tugas KKTKJ*

---

## **0. Pengantar**

Dokumen ini berisi langkah-langkah instalasi dan konfigurasi layanan **Apache2**, **PHP**, **SSL self-signed**, dan **Virtual Host** pada sistem operasi **Debian**.
Panduan dibuat dengan tujuan memberikan referensi teknis yang sistematis, mudah diikuti, dan sesuai kebutuhan praktik administrasi server.

---

## **1. Persiapan Lingkungan**

Sebelum memulai proses instalasi, pastikan bahwa:

1. Server Debian sudah terhubung ke jaringan dan memiliki alamat IP yang dapat diakses.
2. Repository paket pada sistem berfungsi dengan baik.
3. Akses SSH tersedia untuk melakukan administrasi jarak jauh.

---

## **2. Instalasi Apache2**

### **2.1. Update Sistem**

```bash
apt update && apt upgrade -y
```

### **2.2. Instal Paket Apache2**

```bash
apt install apache2 -y
```

### **2.3. Mengaktifkan Layanan Apache2**

```bash
systemctl enable apache2
systemctl start apache2
systemctl status apache2
```

Jika halaman default Apache dapat diakses melalui browser, maka instalasi berhasil.

---

## **3. Instalasi PHP**

Untuk menjalankan aplikasi web dinamis, instal komponen PHP berikut:

```bash
apt install php -y
apt install php-common php-xml php-curl php-zip php-gd php-mbstring php-intl php-json php-soap php-mysql -y
```

---

## **4. Pengujian PHP**

Buat file pengujian:

```bash
nano /var/www/html/info.php
```

Isi dengan:

```php
<?php phpinfo(); ?>
```

Akses melalui browser:

```
http://ip-server/info.php
```

Jika informasi PHP muncul, maka konfigurasi PHP telah berjalan dengan baik.

---

## **5. Konfigurasi SSL Self-Signed**

### **5.1. Instal Modul SSL**

```bash
apt install openssl -y
a2enmod ssl
```

### **5.2. Membuat Direktori Sertifikat**

```bash
mkdir /etc/apache2/ssl
```

### **5.3. Membuat Sertifikat Self-Signed**

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/apache2/ssl/selfsigned.key \
-out /etc/apache2/ssl/selfsigned.crt
```

Isi data sertifikat sesuai kebutuhan.

---

## **6. Konfigurasi Virtual Host HTTPS**

Salin konfigurasi default:

```bash
cp /etc/apache2/sites-available/000-default.conf \
/etc/apache2/sites-available/000-default-ssl.conf
```

Edit file:

```bash
nano /etc/apache2/sites-available/000-default-ssl.conf
```

Isi:

```apache
<VirtualHost *:443>
    ServerAdmin admin@localhost
    DocumentRoot /var/www/html
    ServerName server.local

    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/selfsigned.crt
    SSLCertificateKeyFile /etc/apache2/ssl/selfsigned.key

    <Directory /var/www/html>
        AllowOverride All
        Options Indexes FollowSymLinks
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

---

## **7. Mengaktifkan HTTPS**

```bash
a2ensite 000-default-ssl.conf
a2enmod rewrite
systemctl reload apache2
```

Akses melalui browser:

```
https://ip-server
```

Perangkat biasanya menampilkan peringatan karena sertifikat bersifat self-signed.

---

## **8. Redirect HTTP ke HTTPS**

Edit file:

```bash
nano /etc/apache2/sites-available/000-default.conf
```

Tambahkan:

```apache
Redirect "/" "https://server.local/"
```

Reload Apache:

```bash
systemctl reload apache2
```

---

## **9. Deploy File Web melalui VSCode atau WinSCP**

1. Buka proyek di VSCode.
2. Hubungkan server menggunakan WinSCP.
3. Masukkan direktori:

   ```
   /var/www/html
   ```
4. Upload file proyek.
5. Hapus file `index.html` bawaan Apache jika tidak diperlukan.
6. Akses situs melalui HTTP dan HTTPS.

Jika terdapat masalah permission:

```bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

---

# **Penutup**

Dengan mengikuti langkah-langkah di atas, server Debian Anda kini telah dikonfigurasi dengan:

* Apache2 sebagai web server
* PHP sebagai interpreter aplikasi web dinamis
* SSL self-signed untuk komunikasi terenkripsi
* Virtual host HTTPS
* Redirect otomatis ke protokol aman
* Proses deployment menggunakan VSCode dan WinSCP

Dokumentasi ini dapat dijadikan referensi untuk pengembangan maupun pengelolaan server web berbasis Apache.

---

Jika Anda ingin versi **lebih ringkas**, **lebih teknis**, atau **dalam format PDF**, cukup beri tahu saya.
