# cara install 
1. Login sebagai root atau sudo
2. Beralih ke direktori usr>local>bin dengan perintah :
```sh
cd /usr/local/bin
```
lalu clone git ini dengan perintah : 
```sh
git clone https://github.com/aditbaco/centos7-a2ensite-id_ID.git
```
3. copas file a2ensite dan a2dissite pada folder centos7-a2ensite-id_ID ke direktori /usr/local/bin Anda
```sh
cp centos7-a2ensite-id_ID/centos7-a2ensite ./a2ensssite && cp centos7-a2ensite-id_ID/centos7-a2dissite ./a2dissite
```
kemudian berikan hak akses file dengan perintah :
```sh
chmod +x /usr/local/bin/a2*
```
4. Selanjutnya, buat direktori baru untuk menampung konfigurasi apache webserver kita :
```sh
mkdir /etc/httpd/sites-available && mkdir /etc/httpd/sites-enabled
```
5. Buka configurasi default apache dengan perintah : 
```sh
nano /etc/httpd/conf/httpd.conf
```
Lalu tambahkan baris kode berikut ini di bagian paling akhir: 
```sh
Include sites-enabled/*.conf
```
6. Restart apache : 
```sh
systemctl restart httpd
```

# cara menambah vhost webserver
1. Tambahkan file konfigurasi apache baru berakhiran <website anda>.conf di direktori /etc/httpd/sites-available. Contoh: 
```sh
nano /etc/httpd/sites-available/example.com.conf
```
2. Isikan dengan konfigurasi vhost, bisa pakai standar vhost berikut ini, jangan lupa sesuaikan isiannya. Contoh:
```sh
# Laravel optimized conf example
<VirtualHost *:80>
        ServerName example.com
        ServerAlias www.example.com
        Redirect permanent / https://example.com/
RewriteEngine on
RewriteCond %{SERVER_NAME} =example.com [OR]
RewriteCond %{SERVER_NAME} =www.example.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>

<IfModule mod_ssl.c>
<VirtualHost *:443>
        ServerName example.com
        ServerAlias www.example.com
        SSLEngine On
        DocumentRoot /var/www/example.com/public

        <Directory "/var/www/example.com/public">
                AllowOverride All
                Options FollowSymLinks Multiviews indexes
                RewriteEngine On
                Require all granted
                RewriteCond %{REQUEST_METHOD} ^(TRACE|TRACK)
                RewriteRule .* - [F]
                RewriteCond %{REQUEST_FILENAME} !-f
                RewriteCond %{REQUEST_FILENAME} !-d
                RewriteRule ^(.*)$ index.php?q=$1 [L,QSA]
        </Directory>

        ErrorLog /etc/httpd/logs/example.com-error.log
        CustomLog /etc/httpd/logs/example.com-access.log combined

Include /etc/letsencrypt/options-ssl-apache.conf
SSLCertificateFile /etc/letsencrypt/live/example.com/cert.pem
SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem
SSLCertificateChainFile /etc/letsencrypt/live/example.com/chain.pem
</VirtualHost>
</IfModule>
```
atau gunakan contoh example.com.conf terlampir dengan perintah :
```sh
cp /usr/local/bin/centos7-a2ensite-id_ID/example.com.conf /etc/httpd/sites-available/example.com.conf
```
3. Terakhir, buat symlink dengan perintah :
```sh
a2ensite example.com
```
untuk mematikan website/virtual host tinggal ketikkan perintah :
```sh
a2dissite example.com
```
4. Jangan lupa Restart apache : 
```sh
systemctl restart httpd
```
5. Done. Test URL website baru anda.

Supported by :
Kaisarpropertindo : https://www.kaisarpropertindo.id
Ngenull : https://ngenull.com
Dekate : https://dekate.id
Frilens : https://frilens.id
FNGTMart : https://fngtmart.com
