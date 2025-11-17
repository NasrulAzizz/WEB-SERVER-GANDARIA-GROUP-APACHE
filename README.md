# WEB-SERVER-GANDARIA-GROUP-APACHE
disusun untuk memenuhi tugas KKTKJ

---

# 🌐 Panduan Instalasi Apache2, HTTPS, dan Deploy Project Web di Debian

Dokumen ini berisi langkah-langkah lengkap untuk membangun Web Server Apache2 di Debian, memasang PHP, mengaktifkan HTTPS, hingga upload project web dari VSCode ke server.

---

## 🧩 1. Menyiapkan Debian Server

Pastikan sebelum mulai:

* 🖥️ Debian memiliki IP Address yang dapat diakses LAN
* 📦 Repository sudah disetting
* 🔑 SSH berfungsi (akses lewat CMD / Terminal / WinSCP)

---

## 🏗️ 2. Instalasi Apache Web Server

### 🔄 Update Sistem

```bash
apt update && apt upgrade -y
```

### 🌐 Instal Apache2

```bash
apt install apache2 -y
```

### ▶️ Aktifkan dan Cek Status

```bash
systemctl enable apache2
systemctl start apache2
systemctl status apache2
```

### 🌍 Uji melalui browser

Akses:

```
http://ip-server
```

Jika tampilan default Apache muncul → instalasi sukses.

---

## 🐘 3. Instalasi PHP

### Instal PHP Dasar

```bash
apt install php -y
```

### Instal Modul Tambahan

```bash
apt install php-common php-xml php-curl php-zip php-gd php-mbstring php-intl php-json php-soap php-mysql -y
```

---

## 🔧 4. Memastikan PHP Berjalan

### Buat file info PHP:

```bash
nano /var/www/html/info.php
```

Isi:

```php
<?php phpinfo(); ?>
```

Akses:

```
http://ip-server/info.php
```

Jika halaman info PHP muncul → PHP berhasil.

---

## 🔐 5. Menambahkan SSL (Self-Signed Certificate)

### Instal dan aktifkan SSL

```bash
apt install openssl -y
a2enmod ssl
```

### Buat direktori SSL

```bash
mkdir /etc/apache2/ssl
```

### Generate sertifikat

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/apache2/ssl/selfsigned.key \
-out /etc/apache2/ssl/selfsigned.crt
```

Contoh Identitas yang diisi:

* Country Name: ID
* State: Jawa Barat
* Locality: Bandung
* Organization: Sekolah XYZ
* Organizational Unit: IT Department
* Common Name: serverku.local
* Email: [admin@domain.com](mailto:admin@domain.com)

---

## ⚙️ 6. Konfigurasi Virtual Host HTTPS

### Salin konfigurasi default

```bash
cp /etc/apache2/sites-available/000-default.conf \
/etc/apache2/sites-available/000-default-ssl.conf
```

### Edit konfigurasi SSL

```bash
nano /etc/apache2/sites-available/000-default-ssl.conf
```

Isi dengan:

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

## 🔁 7. Mengaktifkan HTTPS & Modul Rewrite

```bash
a2ensite 000-default-ssl.conf
a2enmod rewrite
systemctl reload apache2
```

Coba akses:

```
https://ip-server
```

(Gembok kuning/merah normal karena sertifikat self-signed)

---

## 🔀 8. Redirect HTTP ke HTTPS (Opsional)

Edit file:

```bash
nano /etc/apache2/sites-available/000-default.conf
```

Tambahkan:

```apache
Redirect "/" "https://server.local/"
```

Reload:

```bash
systemctl reload apache2
```

---

## 🗂️ 9. Deploy Project Web dari VSCode ke Server

1. 📝 Buka folder project web di VSCode
2. 📁 Copy seluruh file (HTML, CSS, JS, PHP, assets)
3. 🔑 Buka WinSCP → login ke server
4. 📂 Masuk ke folder Apache:

   ```
   /var/www/html
   ```
5. 📥 Paste seluruh file project
6. 🗑️ Hapus / ganti file bawaan Apache (mis: index.html)
7. 🌐 Akses website:

```
http://ip-server
https://ip-server
```

Jika muncul masalah permission, jalankan:

```bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

---

## 🎉 Selesai!

Kini server Debian kamu mendukung:

* 🌐 Apache2
* 🐘 PHP + Modul Lengkap
* 🔐 HTTPS (SSL)
* 📄 Virtual Host
* 🔁 Redirect HTTP → HTTPS
* 📂 Deploy project dari VSCode

---
