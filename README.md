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
  <a href="https://youtu.be/ExIepLfq7iU?si=DJ8h-_1GkhVNqSG1">
    <img src="https://youtu.be/ExlepLfq7iU?si=DJ8h-_1GkhVNqSG1" width="500" style="border-radius:10px;">
  </a>
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
````

### 📥 Instal Apache2

```bash
apt install apache2 -y
```

### 🚀 Start & Enable

```bash
systemctl enable apache2
systemctl start apache2
systemctl status apache2
```

✅ Jika halaman default muncul, instalasi sukses.

---

## 🐘 **3. Instalasi PHP**

```bash
apt install php -y
apt install php-common php-xml php-curl php-zip php-gd php-mbstring php-intl php-json php-soap php-mysql -y
```

---

## 🧪 **4. Pengujian PHP**

```bash
nano /var/www/html/info.php
```

Isi:

```php
<?php phpinfo(); ?>
```

Akses via browser:

```
http://ip-server/info.php
```

---

## 🔐 **5. Konfigurasi SSL Self-Signed**

### 📦 Instal Modul SSL

```bash
apt install openssl -y
a2enmod ssl
```

### 📁 Buat folder SSL

```bash
mkdir /etc/apache2/ssl
```

### 🛡️ Generate Sertifikat Self-Signed

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/apache2/ssl/selfsigned.key \
-out /etc/apache2/ssl/selfsigned.crt
```

---

## 🌐 **6. Konfigurasi Virtual Host HTTPS**

```bash
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/000-default-ssl.conf
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
</VirtualHost>
```

---

## 🔓 **7. Mengaktifkan HTTPS**

```bash
a2ensite 000-default-ssl.conf
a2enmod rewrite
systemctl reload apache2
```

Akses: `https://ip-server`
⚠️ Warning SSL normal untuk self-signed.

---

## 🔁 **8. Redirect HTTP ➝ HTTPS**

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

## 📂 **9. Deploy Web via VSCode / WinSCP**

* Upload ke `/var/www/html`
* Hapus `index.html` default
* Jika permission error:

```bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

---

## 🛠️ **Troubleshooting**

Kadang saat setting Apache2, PHP, atau SSL, muncul masalah umum. Berikut beberapa contoh beserta solusinya:

### 1️⃣ **Port 80 Sudah Dipakai**

* **Masalah:** Apache tidak bisa start karena port 80 (HTTP) sudah digunakan aplikasi lain.
* **Cek port:**

```bash
sudo lsof -i :80
```

* **Solusi:**

  * Matikan aplikasi lain yang memakai port 80:

  ```bash
  sudo systemctl stop <nama_layanan>
  ```

  * Atau ubah port Apache di konfigurasi `/etc/apache2/ports.conf`.

---

### 2️⃣ **Forbidden 403 saat Akses Web**

* **Masalah:** Browser menampilkan “403 Forbidden”.
* **Penyebab:** Permission file/folder tidak benar.
* **Solusi:**

```bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

* Pastikan `Directory` di Virtual Host di-set:

```apache
<Directory /var/www/html>
    AllowOverride All
    Options Indexes FollowSymLinks
    Require all granted
</Directory>
```

---

### 3️⃣ **SSL Warning di Browser**

* **Masalah:** Browser menampilkan peringatan “Not Secure” saat akses `https://`.
* **Penyebab:** Sertifikat **self-signed**, bukan resmi dari CA.
* **Solusi:**

  * Peringatan ini normal untuk self-signed. Bisa dilewati dengan “Advanced → Proceed”.
  * Jika ingin aman untuk publik, gunakan **Let’s Encrypt** untuk sertifikat gratis.

---

### 4️⃣ **PHP Tidak Jalan**

* **Masalah:** Halaman `.php` muncul sebagai teks biasa, bukan dijalankan.
* **Penyebab:** Modul PHP belum terinstal atau Apache belum di-reload.
* **Solusi:**

```bash
apt install php libapache2-mod-php -y
systemctl restart apache2
```

---

### 5️⃣ **File Upload / Permission Error**

* **Masalah:** Upload via VSCode/WinSCP gagal atau tidak bisa diakses.
* **Solusi:**

```bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

---

💡 **Tips Umum:**

* Selalu **reload/restart Apache** setelah ubah konfigurasi:

```bash
systemctl reload apache2
systemctl restart apache2
```

* Gunakan log untuk cek error:

```bash
tail -f /var/log/apache2/error.log
```

---


# 🎉 **Penutup**

Server sudah dikonfigurasi:

* ✅ Apache2
* ✅ PHP
* ✅ SSL Self-Signed
* ✅ HTTPS Virtual Host
* ✅ Redirect HTTP → HTTPS
* ✅ Deploy via VSCode / WinSCP

<p align="center" style="color:#00eaff;">✨ Selamat! Server siap digunakan & aman! ✨</p>
