---

<!-- 🌈 GRADIENT BANNER -->
<p align="center">
  <img src="https://svg-banners.vercel.app/api?type=glitch&text1=Apache2%20Setup%20Guide&width=900&height=250" />
</p>

### <p align="center" style="color:#66ccff;">📘✨ <b>Panduan Instalasi & Konfigurasi Apache2 pada Debian</b></p>
<p align="center"><i style="color:#999;">📝 Disusun untuk memenuhi tugas KKTKJ</i></p>

---

<p align="center">
  <img src="https://img.shields.io/badge/Web%20Server-Apache2-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Platform-Debian-red?style=for-the-badge&logo=debian" />
</p>

---

## 📌 **Deskripsi Singkat**
Panduan lengkap instalasi **Apache2**, **PHP**, **SSL**, dan **Virtual Host HTTPS** di Debian. Cocok untuk pembelajaran administrasi server. ⚡

---

## 🎬 **Video Tutorial**
<p align="center">
  <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">
    <img src="https://img.youtube.com/vi/dQw4w9WgXcQ/hqdefault.jpg" width="500" style="border-radius:10px;">
  </a>
</p>

---

## 🖼️ **Screenshot / Foto**
<p align="center">
  <img src="images/apache.png" width="500" style="border-radius:10px;">
</p>

---

## 🧰 **System Requirements**

| Komponen | Minimal | Rekomendasi |
|---------|---------|-------------|
| 💻 OS | Debian 10/11/12 | Debian 12 |
| 🧠 RAM | 512 MB | 1–2 GB |
| 💾 Storage | 5 GB | 10+ GB |
| 🌐 Internet | Ya | Stabil |
| 🔑 Akses | root/sudo | root/sudo |

---

## 🛠️ **1. Persiapan Lingkungan**
Pastikan:  
- 🌐 Server tersambung jaringan  
- 📦 Repository aktif  
- 🔑 Akses sudo/root tersedia  

---

## ⚙️ **2. Instalasi Apache2**
### 🔄 Update Sistem
```bash
apt update && apt upgrade -y

📥 Instal Apache2

apt install apache2 -y

🚀 Start & Enable

systemctl enable apache2
systemctl start apache2
systemctl status apache2

✅ Jika halaman default muncul, instalasi sukses.


---

🐘 3. Instalasi PHP

apt install php -y
apt install php-common php-xml php-curl php-zip php-gd php-mbstring php-intl php-json php-soap php-mysql -y


---

🧪 4. Pengujian PHP

nano /var/www/html/info.php

Isi:

<?php phpinfo(); ?>

Akses via browser:

http://ip-server/info.php


---

🔐 5. Konfigurasi SSL Self-Signed

📦 Instal Modul SSL

apt install openssl -y
a2enmod ssl

📁 Buat folder SSL

mkdir /etc/apache2/ssl

🛡️ Generate Sertifikat Self-Signed

openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/apache2/ssl/selfsigned.key \
-out /etc/apache2/ssl/selfsigned.crt


---

🌐 6. Konfigurasi Virtual Host HTTPS

cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default-ssl.conf
nano /etc/apache2/sites-available/000-default-ssl.conf

Isi:

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
</VirtualHost>


---

🔓 7. Mengaktifkan HTTPS

a2ensite 000-default-ssl.conf
a2enmod rewrite
systemctl reload apache2


---

🔁 8. Redirect HTTP ➝ HTTPS

nano /etc/apache2/sites-available/000-default.conf

Tambahkan:

Redirect "/" "https://server.local/"

Reload:

systemctl reload apache2


---

📂 9. Deploy Web via VSCode / WinSCP

chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html


---

🛠️ Troubleshooting

1️⃣ Port 80 Dipakai

sudo lsof -i :80

2️⃣ Forbidden 403

chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html

3️⃣ SSL Warning

Normal untuk self-signed.

4️⃣ PHP Tidak Jalan

apt install php libapache2-mod-php -y
systemctl restart apache2

5️⃣ Permission Error Upload

chmod -R 755 /var/www/html


---

🎉 Penutup

<p align="center" style="color:#00eaff;">✨ Selamat! Server siap digunakan & aman! ✨</p>
```
---
