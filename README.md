# Jarkom-Modul-2-B21-2023

# Anggota Kelompok

| **No** | **Nama**                         | **NRP**    |
| ------ | -------------------------------- | ---------- |
| 1      |Anneu Tsabita Putri               | 5025211026 |
| 2      | Cavel Ferrari                    | 5025211198 |

# Dokumentasi Pengerjaan Soal

## Nomor 1
`Yudhistira` akan digunakan sebagai DNS Master, `Werkudara` sebagai DNS Slave, `Arjuna` merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu `Prabakusuma`, `Abimanyu`, dan `Wisanggeni`. Buatlah topologi dengan pembagian yang dapat diakses pada drive pada soal.

## Jawaban
Pertama, membuat topologi sesuai permintaan soal. Kelompok kami mendapat topologi 2, berikut topologinya:
![topologi](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/cd4db9e2-4c94-47f6-acea-740cd5a2d56f)

Kemudian *setting network* masing-masing *node* dengan fitur `Edit network configuration` yang ada di menu `Configure`. *Setting* awal yang sudah ada dapat dihapus dan diganti dengan konfigurasi berikut:

- Pandudewanata
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
	address 10.19.1.1
	netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
	address 10.19.2.1
	netmask 255.255.255.0

  auto eth3
  iface eth3 inet static
	address 10.19.3.1
	netmask 255.255.255.0
  ```

- Sadewa
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.1.2
	netmask 255.255.255.0
	gateway 10.19.1.1
  ```
  
  - Nakula
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.1.3
	netmask 255.255.255.0
	gateway 10.19.1.1
  ```
    
- Yudhistira
  ```
  auto eth0
  iface eth0 inet static
        address 10.19.2.2
	netmask 255.255.255.0
	gateway 10.19.2.1
  ```
  
- Werkudara
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.3.2
	netmask 255.255.255.0
	gateway 10.19.3.1
  ```
  
- Arjuna
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.3.3
	netmask 255.255.255.0
	gateway 10.19.3.1
  ```
  
- Abimanyu
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.3.4
	netmask 255.255.255.0
	gateway 10.19.3.1
  ```
  
- Prabakusuma
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.3.5
	netmask 255.255.255.0
	gateway 10.19.3.1
  ```
  
- Wisanggeni
  ```
  auto eth0
  iface eth0 inet static
	address 10.19.3.6
	netmask 255.255.255.0
	gateway 10.19.3.1
  ```

### Pandudewanata

*Restart* semua *node*. Lalu jalankan *command* berikut pada *router* `Pandudewanata` untuk pengaturan lalu lintas komputer.
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.19.0.0/16
```
(Note: *Prefix* IP yang digunakan sesuai *Prefix* IP Kelompok, dalam hal ini kelompok B21 adalah **10.19**).

Dan ketikkan *command* ini pada `Pandudewanata` untuk melihat IP DNS:
```
cat /etc/resolv.conf
```
Akan muncul *nameserver* yang akan digunakan pada konfigurasi selanjutnya.

![cat](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/03f80533-8ac2-43ba-90d5-d4fd471f7344)

### Semua node (kecuali Pandudewanata)

Agar *node*-*node* lainnya dapat mengakses internet, jalankan *command* berikut dan gunakan IP DNS dari `Pandudewanata` tadi.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

*Restart* semua *node* kembali. Lalu, *testing* semua *node* apakah sudah terkoneksi dengan internet dengan `ping` ke [**google.com**](google.com). Sebagai contoh pada `Sadewa`:

![pinggoogle](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/b2e300cf-42a2-4f20-a1f8-9173e42cb73e)

## Nomor 2
Buatlah website utama pada node arjuna dengan akses ke ``arjuna.yyy.com`` dengan alias ``www.arjuna.yyy.com`` dengan ``yyy`` merupakan kode kelompok.

## Jawaban
Berikut langkah-langkah untuk membuat domain arjuna.b21.com.

### Yudhistira

Melakukan instalasi **bind9** terlebih dahulu pada `Yudhistira` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install bind9 -y
```

Setelah instalasi selesai, buat domain [**arjuna.yyy.com**](arjuna.yyy.com). Lakukan *command* seperti berikut pada `Yudhistira`.
```
nano /etc/bind/named.conf.local
```

Isi konfigurasi domain [**arjuna.yyy.com**](arjuna.yyy.com) sesuai sintaks berikut.
```
zone "arjuna.b21.com" {
    type master;
    file "/etc/bind/jarkom/arjuna.b21.com";
};
```

![confarjuna](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/01dbe1ac-5181-4778-80e9-55b0a1c3ce19)

Buat folder baru, yaitu **jarkom** pada **/etc/bind**.
```
mkdir /etc/bind/jarkom
```

*Copy file* **db.local** ke dalam folder **jarkom** yang baru dibuat dan ubah namanya menjadi [**arjuna.b21.com**](arjuna.b21.com).
```
cp /etc/bind/db.local /etc/bind/jarkom/arjuna.b21.com
```

Buka *file* [**arjuna.b21.com**](arjuna.b21.com) dan edit seperti konfigurasi berikut.

![arjuna](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/7032e31c-67df-4b7f-8991-ed8039cff0ba)

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.arjuna.b21.com**](www.arjuna.b21.com) untuk membuat alias yang mengarahkan domain ke [**arjuna.b21.com**](arjuna.b21.com).

*Restart* **bind9**.
```
service bind9 restart
```

### Sadewa atau Nakula

Lakukan *testing* dengan menambahkan `nameserver 10.19.2.2` (IP Yudhistira) pada `Sadewa` dan `Nakula` untuk cek apakah [**arjuna.b21.com**](arjuna.b21.com) atau [**www.arjuna.b21.com**](www.arjuna.b21.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

![pingarjuna](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/7340f68c-9234-4a06-8f0f-257590c390a8)

![pingwwwarjuna](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/5eeff522-1f49-4f02-b7a0-93bcd457b2ef)

## Nomor 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke `abimanyu.yyy.com` dan alias `www.abimanyu.yyy.com`.

## Jawaban
Untuk membuat domain `abimanyu.yyy.com` dan alias `www.abimanyu.yyy.com`, langkah-langkah yang dilakukan sama seperti pengerjaan ada nomor 2. Jika sukses, maka akan memunculkan hasil seperti berikut.

![pingabimanyu](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/2714e4da-1e8b-42fb-bc2a-89bc3912c7e0)

![pingwwwabimanyu](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/fb55d97b-068a-4a52-822c-5d983030e58a)

## Nomor 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain `parikesit.abimanyu.yyy.com` yang diatur DNS-nya di `Yudhistira` dan mengarah ke `Abimanyu`.

## Jawaban

### Yudhistira

Buka *file* [**abimanyu.b21.com**](abimanyu.b21.com) dan edit seperti konfigurasi berikut.

![abimanyu](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/a9a1f3fb-a07d-490a-baa4-dccafdaa1c02)

*Restart* **bind9**.
```
service bind9 restart
```

### Sadewa atau Nakula

Lakukan *testing* pada `Sadewa` dan `Nakula` untuk cek apakah [**abimanyu.b21.com**](abimanyu.b21.com) atau [**www.abimanyu.b21.com**](www.abimanyu.b21.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

![pingparikesit](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/59f8175e-4c34-458e-9202-8140180463a1)

## Nomor 5
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse).

## Jawaban

### Yudhistira

Edit *file* **/etc/bind/named.conf.local** pada `Yudhistira` dan tambahkan konfigurasi berikut.  Tambahkan *reverse* dari 3 *bytes* awal dari IP yang ingin dilakukan **Reverse DNS**. Dalam hal ini IP **10.19.3** untuk IP dari *record* sehingga *reverse*-nya adalah **3.19.10**.
```
zone "3.19.10.in-addr.arpa" {
    type master;
    file "/etc/bind/jarkom/3.19.10.in-addr.arpa";
};
```

*Copy file* **db.local** ke dalam folder **jarkom** dan ubah namanya menjadi [**3.19.10.in-addr.arpa**](3.19.10.in-addr.arpa).
```
cp /etc/bind/db.local /etc/bind/jarkom/3.19.10.in-addr.arpa
```

Buka *file* [**3.19.10.in-addr.arpa**](3.19.10.in-addr.arpa) dan edit seperti konfigurasi berikut.

![reverse](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/6a562257-c27d-43f4-a659-25775f4c5d9c)

*Restart* **bind9**.

```
service bind9 restart
```

### Sadewa atau Nakula

Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada `Sadewa`.
```
// Install package dnsutils, ubah nameserver ke 192.168.122.1
apt-get update
apt-get install dnsutils -y

// Kembalikan nameserver agar tersambung dengan Yudhistira
host -t PTR 10.19.2.2
```

Akan muncul seperti ini.

![reverselg](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/a5794647-fcb4-4737-ac36-c113ff2a8e05)

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
lynx www.parikesit.abimanyu.b21.com/public/images/uwogh.jpg
```


