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
3. Selanjutnya, buat direktori baru untuk menampung konfigurasi apache webserver kita :
```sh
mkdir /etc/httpd/sites-available && mkdir /etc/httpd/sites-enabled
```
4. Buka configurasi default apache dengan perintah : 
```sh
nano /etc/httpd/conf/httpd.conf
```
Lalu tambahkan baris kode berikut ini di bagian paling akhir: 
```sh
Include sites-enabled/*.conf
```
5. Restart apache : 
```sh
systemctl restart httpd
```

# cara menambah vhost webserver
1. Tambahkan file konfigurasi apache baru berakhiran <website anda>.conf di direktori /etc/httpd/sites-available. Contoh: 
```sh
nano /etc/httpd/sites-available/websiteku.id.conf
```
2. Isikan dengan konfigurasi vhost, bisa pakai standar vhost berikut ini, jangan lupa sesuaikan isiannya. Contoh:
```sh
<VirtualHost *:80>
        ServerName websiteku.id
        ServerAlias www.websiteku.id
        #AllowOverride All
        ServerAdmin admin@localhost
        DocumentRoot /var/www/websiteku.id
        ErrorLog /etc/httpd/logs/websiteku.id-error.log
        CustomLog /etc/httpd/logs/websiteku.id-access.log combined

        # This is for Drupal8 clean URLs.
        <Directory /var/www/websiteku.id>
		Options Indexes FollowSymLinks MultiViews
                RewriteEngine On
                RewriteBase /
                RewriteCond %{REQUEST_FILENAME} !-f
                RewriteCond %{REQUEST_FILENAME} !-d
                RewriteRule ^(.*)$ index.php?q=$1 [L,QSA]
        </Directory>
		<IfModule mpm_peruser_module>
                ServerEnvironment apache apache
        </IfModule>
</VirtualHost>
```
3. Terakhir, buat symlink dengan perintah :
```sh
a2ensite websiteku.id
```
4. Restart apache : 
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
