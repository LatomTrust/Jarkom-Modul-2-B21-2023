# Jarkom-Modul-2-B21-2023

# Anggota Kelompok

| **No** | **Nama**                         | **NRP**    |
| ------ | -------------------------------- | ---------- |
| 1      |Anneu Tsabita Putri               | 5025211026 |
| 2      | Cavel Ferrari                    | 5025211198 |

# Dokumentasi Pengerjaan Soal

## Nomor 11
### Soal
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Jawaban
melakukan instalasi apache 
```
apt-get install apache2
service apache2 start
```
kemudian pada `/etc/apache2/sites-available/000-default.conf` ditambahkan sebgai berikut

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com

        ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

```

kemudian dilakukan restart
```
apt-get install libapache2-mod-php7.0
service apache2 restart
```

kemudian dilakukan testing pada client
```
lynx abimanyu.b21.com
```
<img width="601" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/e832347d-1c91-487d-a216-50e46112d7f5">



## Nomor 12
### Soal
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### Jawaban
tambahkan alias pada `/etc/apache2/sites-available/000-default.conf` sebagai berikut

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"
    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

```
kemudian dilakukan restart
```
service apache2 restart
```

kemudian dilakukan testing pada client
```
lynx abimanyu.b21.com/home
```
diperoleh sebagai berikut

<img width="601" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/e832347d-1c91-487d-a216-50e46112d7f5">


## Nomor 13
### Soal
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Jawaban

 pada `/etc/apache2/sites-available/000-default.conf` tambahkan konfigurasi sebagai berikut

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com

     ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>
```
kemudian restart apache
```
service apache2 restart
```
untuk testing dapat dilakukan `lynx parikesit.abimanyu.b21.com` diperoleh sebagai berikut

<img width="502" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/ecaebde8-06df-4e34-a604-4491cea9a85a">


## Nomor 14
### Soal

Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

### Jawaban
pada parikesit.abimanyu.b21.com di `/etc/apache2/sites-available/000-default.conf` dapat ditambahkan `Deny From All` sebagai berikut

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

```

kemudian restart apache
```
service apache2 restart
```

dan dilakukan testing pada client dengan 
```
lynx parikesit.abimanyu.b21.com/secret
```
diperoleh

<img width="509" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/19ebab28-5884-4684-829f-94544adb557d">

## Nomor 15
### Soal
Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

### Jawaban
pada parikesit.abimanyu.b21.com di `/etc/apache2/sites-available/000-default.conf` dapat ditambahkan jenis error dan path menuju file error kustomnya.

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"
    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
        </Directory>
        ErrorDocument403 /error/403.html
        ErrorDocument 404 /error/404.html
     ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

```
kemudian restart apache
```
service apache2 restart
```

untuk melakukan testing untuk 2 kasus:
1. error 403 (error forbidden)
dengan menggunakan `lynx parikesit.abimanyu.b21.com/secret` diperoleh tampilan

<img width="506" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/25a4190e-9028-48dc-b568-e27f7cbfeb1f">

2. error 403 (error not found)
dengan menggunakan `lynx parikesit.abimanyu.b21.com/kani` diperoleh tampilan


<img width="505" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/5f6fba50-d938-421e-985f-1529619d0da4">


## Nomor 16
### Soal
Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi www.parikesit.abimanyu.yyy.com/js 

### Jawaban
menambahkan alias pada parikesit.abimanyu.b21.com di `/etc/apache2/sites-available/000-default.conf` sebagai berikut:

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
        </Directory>
        Alias "/js" "/var/www/parikesit.abimanyu.b21/public/js"
        ErrorDocument403 /error/403.html
        ErrorDocument 404 /error/404.html
       ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>
```
kemudian restart apache
```
service apache2 restart
```
kemudian untuk testing dapat dilakukan dengan `lynx www.parikesit.abimanyu.b21.com/js` diperoleh

<img width="503" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/21d631fa-3a3e-49b5-86e2-60c6576f5cf9">


## Nomor 17
### Soal
Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

### Jawaban
tambahkan konfigurasi rjp.baratayuda.abimanyu.b21.com kedalam file `/etc/apache2/sites-available/000-default.conf` namun dengan port 14000 dan 14400.

```

VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

     ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
        </Directory>
        Alias "/js" "/var/www/parikesit.abimanyu.b21/public/js"
        ErrorDocument403 /error/403.html
        ErrorDocument 404 /error/404.html
        ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined

</VirtualHost>

<VirtualHost *:14000 *:14400>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.b21
        ServerName rjp.baratayuda.abimanyu.b21.com
        ServerALias www.rjp.baratayuda.abimanyu.b21.com

    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined

</VirtualHost>

```

kemudian tambahkan port 14000 dan 14400 pada konfigurasi port `/etc/apache2/ports.conf` sebagai berikut:

```
Listen 80
Listen 14000
Listen 14400

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

```
kemudian restart apache
```
service apache2 restart
```
untuk testing dapat dilakukan 

```
lynx rjp.baratayuda.abimanyu.b21.com:14000
```
dan

```
lynx rjp.baratayuda.abimanyu.b21.com:14400
```

## Nomor 18
### Soal
Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

### Jawaban
setting username dan password pada suatu folder
```
mkdir /etc/apache2/passwd
htpasswd -c -b /etc/apache2/passwd/password Wayang baratayudab21
```
kemudian pada rjp.baratayuda.abimanyu.b21.com di `/etc/apache2/sites-available/000-default.conf` ditambahkan sebagai berikut

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>

        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
        </Directory>
        Alias "/js" "/var/www/parikesit.abimanyu.b21/public/js"
        ErrorDocument403 /error/403.html
        ErrorDocument 404 /error/404.html
      ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<VirtualHost *:14000 *:14400>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.b21
        ServerName rjp.baratayuda.abimanyu.b21.com
        ServerALias www.rjp.baratayuda.abimanyu.b21.com
        <Directory/var/www/rjp.baratayuda.abimanyu.b21>
                AuthType Basic
                AuthName "Restricted Files"
                # (Following line optional)
                AuthBasicProvider file
                AuthUserFile "/etc/apache2/passwd/password"
                Require user Wayang
        </Directory>
       ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined

</VirtualHost>

```

kemudian restart apache
```
service apache2 restart
```

sehingga jika dilakukan testing pada port 14000
```
lynx www.rjp.baratayuda.abimanyu.b21.com:14000
```
sehingga diperoleh
![image](Images/no18b.png)

![image](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/c684b6cf-e261-48bd-886c-4428fc60d8e3)

<img width="500" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/76f643aa-2e59-486e-863c-f88b1da5cbd6">


## Nomor 19
### Soal
Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

### Jawaban
melakukan enable pada rewrite dengan
```
a2enmod rewrite
```
pada abimanyu.b21.com di `/etc/apache2/sites-available/000-default.conf` dapat ditambahkan RewriteRule dan RewriteCond sebagai berikut.

```

VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>
        <Directory /var/www>
        RewriteEngine on
        RewriteCond %{HTTTP_HOST} ^10\.19\.3.4$
        RewriteRule /.* http://www.abimanyu.b21.com/index.php/home."
        </Directory>
        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
        </Directory>
        Alias "/js" "/var/www/parikesit.abimanyu.b21/public/js"
        ErrorDocument403 /error/403.html
        ErrorDocument 404 /error/404.html
    ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined


</VirtualHost>

<VirtualHost *:14000 *:14400>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.b21
        ServerName rjp.baratayuda.abimanyu.b21.com
        ServerALias www.rjp.baratayuda.abimanyu.b21.com
        <Directory/var/www/rjp.baratayuda.abimanyu.b21>
                AuthType Basic
                AuthName "Restricted Files"
                # (Following line optional)
                AuthBasicProvider file
                AuthUserFile "/etc/apache2/passwd/password"
                Require user Wayang
        </Directory>
           ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined

</VirtualHost>

```
kemudian restart apache
```
service apache2 restart
```
terakhir dilakukan testing dengan

```
lynx 10.19.3.4
```
atau
```
lynx www.abimanyu.b21.com
```

<img width="506" alt="image" src="https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/91776952/433a341d-4d24-4083-b81d-a47c59c7fdd5">


## Nomor 20
### Soal
Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

### Jawaban
melakukan enable pada rewrite dengan
```
a2enmod rewrite
```
pada parikesit.abimanyu.b21.com di `/etc/apache2/sites-available/000-default.conf` dapat ditambahkan redirect pada request yang memiliki kata abimanyu dan extension jpg atau png.

```
VirtualHost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/abimanyu.b21.com
        ServerName abimanyu.b21.com
        ServerAlias www.abimanyu.b21.com
        <Directory/var/www/abimanyu.b21.com/index.php/home>
                Options +Indexes
        </Directory>
        <Directory /var/www>
        RewriteEngine on
        RewriteRule ^public/images/abimanyu\.png$ - [L]
        RewriteCond %{REQUEST_URI} abimanyu
        RewriteRule \.(jpg|png)$ /public/images/abimanyu.png [R=301,L]
        </Directory>
        Alias"/home""/var/www/abimanyu.b21.com/index.php/home"

        ErrorLog $EAPACHE_LOG_DIR]/error.Log
        CustomLog $[APACHE_LOG_DIR]/access.log combined

</VirtualHost>

<Virtualhost *:80>

        ServerAdmin webmaster@localhost
        DocumentRoot/var/www/parikesit.abimanyu.b21
        ServerName parikesit.abimanyu.b21.com
        ServerALiaswww.parikesit.abimanyu.b21.com
        <Directory /var/www/parikesit.abimanyu.b21/secret>
                Deny From All
        </Directory>
        Alias "/js" "/var/www/parikesit.abimanyu.b21/public/js"
        ErrorDocument403 /error/403.html
        ErrorDocument 404 /error/404.html
        ErrorLog ${APACHE_LOG_DIR} /error. Log
        CustomLog ${APACHE_LOG_DIR}/access.Log combined

</VirtualHost>

<VirtualHost *:14000 *:14400>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.b21
        ServerName rjp.baratayuda.abimanyu.b21.com
        ServerALias www.rjp.baratayuda.abimanyu.b21.com
        <Directory/var/www/rjp.baratayuda.abimanyu.b21>
                AuthType Basic
                AuthName "Restricted Files"
                # (Following line optional)
                AuthBasicProvider file
                AuthUserFile "/etc/apache2/passwd/password"
                Require user Wayang
        </Directory>
        ErrorLog $EAPACHE._LOG_DIR /error.Log
        CustomLog $[APACHE_LOG_DIR]/access.log combined
</VirtualHost>
```
kemudian restart apache
```
service apache2 restart
```
test dengan

```
lynx www.parikesit.abimanyu.E06.com/public/images/uwogh.jpg
```


