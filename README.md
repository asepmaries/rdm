Versi Hosting
Versi ini kami siapkan untuk teman-teman oprator madrasah yang menggunakan Hosting atau VPS sehingga guru-guru dapat mengakses aplikasi rapor dari mana saja..
Selain itu, untuk versi Hosting, ada 2 metode yang kami siapkan, yaitu full source dan mini install.
Untuk versi mini install, teman-teman cukup download source yang cukup kecil, dan kemudian upload dan extract ke folder hosting atau vps. Untuk panduannya dapat teman-teman lihat di video tutorial.
Sarat Hosting:

- PHP 7.2
- Ioncube Loader php7.2
- Allow url_fopen dari php.ini
- Curl Aktif
- Resource Hosting cukup untuk pengguna Rapor Anda

Versi RDM Madrasah
Kami menyiapkan 3 versi RDM yang dapat teman-teman pilih sesuai dengan kebutuhan teman masing-masing.
Aplikasi RDM tidak diperjual-belikan (Token dan aplikasi gratis).
Madrasah bisa menggunakan versi XAMPP dan VDI secara gratis, akan tetapi jika ada madrasah yang mengonlinekan RDM dengan menggunakan jasa Hosting, maka yang diperjual-belikan adalah hostingnya dan pihak pengembang tidak memfasilitasi penyediaan Hosting untuk RDM sehingga madrasah bisa menggunakan jasa pihak lain.
Jadi jika ada pihak yang menjual Aplikasi RDM, harap melaporkan ke HD Kab/Provinsi.

Info Terkait Update RDM dapat dilihat pada channel Telegram 
Untuk yang tidak bisa melakukan update secara online


1. Persiapan VPS
Login ke VPS: Gunakan SSH untuk login ke VPS Anda.
bash
Salin kode
    ssh username@your_vps_ip
Update dan Upgrade Sistem:
bash
Salin kode

`sudo apt update
sudo apt upgrade -y`
3. Install Nginx
Install Nginx:
bash
Salin kode
`sudo apt install nginx -y`
Start dan Enable Nginx:
bash
Salin kode
sudo systemctl start nginx
sudo systemctl enable nginx
4. Install PHP 7.2 dan Ekstensi Standar
Tambahkan PPA untuk PHP 7.2:
bash
Salin kode
sudo add-apt-repository ppa:ondrej/php
sudo apt update
Install PHP 7.2 dan Ekstensi:
bash
Salin kode
sudo apt install php7.2 php7.2-fpm php7.2-cli php7.2-mysql php7.2-xml php7.2-mbstring php7.2-curl php7.2-zip php7.2-gd php7.2-soap -y
Start dan Enable PHP-FPM:
bash
Salin kode
sudo systemctl start php7.2-fpm
sudo systemctl enable php7.2-fpm
5. Install Ioncube Loader
Download Ioncube Loader:
bash
Salin kode
wget https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz
Extract Ioncube Loader:
bash
Salin kode
tar xvfz ioncube_loaders_lin_x86-64.tar.gz
Copy Ioncube Loader ke Direktori PHP:
bash
Salin kode
sudo cp ioncube/ioncube_loader_lin_7.2.so /usr/lib/php/20170718/
Edit PHP Configuration:
Tambahkan baris berikut pada file konfigurasi PHP:
bash
Salin kode
sudo nano /etc/php/7.2/fpm/php.ini
Tambahkan di bagian paling atas atau bagian Dynamic Extensions:
bash
Salin kode
zend_extension = /usr/lib/php/20170718/ioncube_loader_lin_7.2.so
Restart PHP-FPM:
bash
Salin kode
sudo systemctl restart php7.2-fpm
6. Allow url_fopen dan Aktifkan Curl
Edit PHP Configuration:
bash
Salin kode
sudo nano /etc/php/7.2/fpm/php.ini
Cari dan pastikan opsi berikut diaktifkan:
Untuk allow_url_fopen, pastikan nilainya On:
ini
Salin kode
allow_url_fopen = On
Untuk curl, pastikan modul curl terinstall dan aktif.
Restart PHP-FPM:
bash
Salin kode
sudo systemctl restart php7.2-fpm
7. Clone Repository ke VPS
Install Git:
bash
Salin kode
sudo apt install git -y
Clone Repository:
bash
Salin kode
git clone https://github.com/asepmaries/rdm.git /var/www/html/rdm
Set Permissions:
bash
Salin kode
sudo chown -R www-data:www-data /var/www/html/rdm
sudo chmod -R 755 /var/www/html/rdm
8. Konfigurasi Nginx
Buat Konfigurasi Baru untuk Situs Anda:
bash
Salin kode
sudo nano /etc/nginx/sites-available/rdm
Tambahkan konfigurasi berikut:
nginx
Salin kode
server {
    listen 80;
    server_name your_domain_or_ip;

    root /var/www/html/rdm;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
Aktifkan Konfigurasi dan Restart Nginx:
bash
Salin kode
sudo ln -s /etc/nginx/sites-available/rdm /etc/nginx/sites-enabled/
sudo systemctl restart nginx
9. Selesai
