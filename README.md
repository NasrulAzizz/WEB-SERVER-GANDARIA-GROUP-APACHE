# WEB-SERVER-GANDARIA-GROUP-APACHE
disusun untuk memenuhi tugas KKTKJ

---

# ██🔥💀🔥💀🔥 ACAB APACHE SERVER GUIDE 🔥💀🔥💀🔥██

### *WEB-SERVER-GANDARIA-GROUP-APACHE*

### *Dokumen ini dibuat bukan untuk patuh—tapi untuk MEMBANGKANG.*

### **ACAB = All Cops Are Blockers (penghalang server)**

> “Jika sistem memata-matai kita, kita bangun server sendiri.”
> — Anarko Sysadmin, 2003

---

# 🏴‍☠️🌐 PANDUAN INSTALASI APACHE2 (ANTI-OTORITAS EDITION)

**Debian?**
Kita siksa.
**Apache?**
Kita paksa bekerja.
**HTTPS?**
Kita bikin versi bajakan.
**ACAB.**

---

# 🚫👮 0. DISCLAIMER (VERSI PUNK)

**Dokumen ini tidak tunduk pada standar mana pun.**
Tidak ISO. Tidak SOP.
Tidak ada polisi IT yang mengatur.
**ACAB, termasuk "sysadmin polisi" yang suka nge-ceklist.**

---

## 🧩🔥 1. PERSIAPAN — *SIAPKAN SENJATA DIGITAL*

Sebelum ngebut:

* 🖥️ Debian punya IP LAN → **buat kita obrak-abrik**
* 📦 Repo hidup → **biar download paket jadi cepat kayak kerusuhan**
* 🔑 SSH aktif → **akses ilegal tapi legal karena punya server sendiri**

---

## 🤘🔥 2. INSTAL APACHE — *BANGUNKAN DEMON DI DALAM SERVER*

**Kita mulai bakar.**

### 🔄 Update Sistem

```bash
apt update && apt upgrade -y
```

### 🔥 Install Apache2

```bash
apt install apache2 -y
```

### 🚬 Aktifkan & Lihat Napasnya

```bash
systemctl enable apache2
systemctl start apache2
systemctl status apache2
```

Kalau tampil halaman default:
→ **SERVER MUDAH SEKALI TAKLUK.**

---

## 💀🐘 3. INSTALL PHP — *BIAR SERVER PUNYA OTAK*

Tanpa PHP, server cuma zombie jalan.

```bash
apt install php -y
apt install php-common php-xml php-curl php-zip php-gd php-mbstring php-intl php-json php-soap php-mysql -y
```

---

## ⚰️ 4. TEST PHP — *BIAR GAK DIBOHONGI SISTEM*

Bikin file:

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

Jika muncul → **SUCCESS, F**K OTORITAS**.**

---

## 🔐💣 5. SSL (SELF-SIGNED) — *KITA GAK PERCAYA OTORITAS SERTIFIKAT*

**Kita bikin sertifikat sendiri.**
Karena **CA = Cops Authority**, dan **ACAB**.

### Instal SSL

```bash
apt install openssl -y
a2enmod ssl
```

### Direktori ilegal

```bash
mkdir /etc/apache2/ssl
```

### Generate Sertifikat Bajakan

```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout /etc/apache2/ssl/selfsigned.key \
-out /etc/apache2/ssl/selfsigned.crt
```

Isi identitas:
→ **bebas, semua CA bisa ke neraka.**

---

## ⚙️🕳️ 6. VIRTUAL HOST — *KITA BUAT WILAYAH SERVER SENDIRI*

Copy:

```bash
cp /etc/apache2/sites-available/000-default.conf \
/etc/apache2/sites-available/000-default-ssl.conf
```

Edit:

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

> **Setiap baris config adalah aksi perlawanan.**

---

## 🔥🔁 7. NYALAKAN HTTPS — *TENDANG SERVICENYA SAMPAI BANGUN*

```bash
a2ensite 000-default-ssl.conf
a2enmod rewrite
systemctl reload apache2
```

Akses:

```
https://ip-server
```

Gembok merah?
→ **BIAR.**
Itu simbol **PERLAWANAN**.

---

## 🚧⚡ 8. REDIRECT HTTP → HTTPS

Edit:

```bash
nano /etc/apache2/sites-available/000-default.conf
```

Tambah:

```apache
Redirect "/" "https://server.local/"
```

Reload:

```bash
systemctl reload apache2
```

---

## 🗂️🔥 9. DEPLOY VIA VSCODE — *DROP PAYLOAD KE TARGET*

Langkah-langkah sabotase:

1. Buka project di VSCode
2. Copy semua file
3. Login WinSCP
4. Masuk:

   ```
   /var/www/html
   ```
5. Paste
6. Hapus index.html culun bawaan Apache
7. Akses:

```
http://ip-server
https://ip-server
```

Permission ngamuk?

```bash
chmod -R 755 /var/www/html
chown -R www-data:www-data /var/www/html
```

---

# 🎉🔥💀 SERVER AKHIRNYA JADI MESIN ANARKI

Server Debian kamu sekarang:

* 🏴 Apache2 jalan
* 🏴 PHP liar
* 🏴 HTTPS bajakan
* 🏴 Virtual host custom
* 🏴 Redirect paksa
* 🏴 Deploy barbar via VSCode

Dan yang paling penting:

### **🏴 ACAB — ALL CERTIFICATE AUTHORITIES BURN 🔥**

### **🏴 ACAB — ALL CONFIG APPROVAL BUREAUS DIE 💥**

### **🏴 ACAB — ALL CONTROL ARE BULLSH*T ⚡**

---

### “NAIKKAN LEVEL ANARKI.”
