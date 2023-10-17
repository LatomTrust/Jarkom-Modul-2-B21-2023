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

## Nomor 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

## Jawaban

### Yudhistira

Modifikasi konfigurasi berikut pada **/etc/bind/named.conf.local** di `Yudhistira`.
```
zone "arjuna.b21.com" {
    type master;
    notify yes;
    also-notify { 10.19.3.2; }; // IP Werkudara
    allow-transfer { 10.19.3.2; }; // IP Werkudara
    file "/etc/bind/jarkom/arjuna.b21.com";
};

zone "abimanyu.b21.com" {
    type master;
    notify yes;
    also-notify { 10.19.3.2; }; // IP Werkudara
    allow-transfer { 10.19.3.2; }; // IP Werkudara
    file "/etc/bind/jarkom/abimanyu.b21.com";
};
```

*Restart* **bind9**.
```
service bind9 restart
```

### Werkudara

Melakukan instalasi **bind9** pada `Werkudara` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install bind9 -y
```

Tambahkan konfigurasi berikut pada **/etc/bind/named.conf.local** di `Werkudara`.
```
zone "arjuna.b21.com" {
    type slave;
    masters { 10.3.2.2; }; // IP Yudhistira
    file "/var/lib/bind/arjuna.b21.com";
};

zone "abimanyu.b21.com" {
    type slave;
    masters { 10.3.2.2; }; // IP Yudhistira
    file "/var/lib/bind/abimanyu.b21.com";
};
```

*Restart* **bind9**.
```
service bind9 restart
```

### Yudhistira

Lakukan *testing* pada `Sadewa` dan `Nakula` untuk cek apakah **DNS Slave** berhasil dibuat pada `Werkudara`. *Stop service* bind9 terlebih dahulu pada `Yudhistira`.
```
service bind9 stop
```

### Sadewa atau Nakula

Pada `Sadewa` dan `Nakula` jangan lupa untuk menambahkan *nameserver* `Werkudara`, yaitu **10.19.3.2** pada **/etc/resolv.conf**, sehingga menjadi seperti ini:

![rresolc](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/84a73194-18ba-4856-8837-a1a055e223e4)

Hasilnya akan seperti di bawah ketika dijalankan di `Sadewa`:

![slave](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/5aa2bbf0-1a3f-41c6-a08e-5db6becd2747)

## Nomor 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu `baratayuda.abimanyu.yyy.com` dengan alias `www.baratayuda.abimanyu.yyy.com` yang didelegasikan dari `Yudhistira` ke `Werkudara` dengan IP menuju ke `Abimanyu` dalam folder `Baratayuda`.

## Jawaban

### Yudhistira

Buka *file* [**abimanyu.b21.com**](abimanyu.b21.com) dan edit seperti konfigurasi berikut.

![abi2](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/df393729-658d-43e0-b78e-ececdc49acca)

Buka *file* **/etc/bind/named.conf.options** dan edit seperti konfigurasi berikut. *Comment* bagian `dnssec-validation auto` dan tambahkan di baris bawahnya `allow-query{any;}`.

![allowquery](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/a068fae0-7088-4d45-b286-e5c2c0ca16b0)

### Werkudara

Tambahkan konfigurasi berikut pada **/etc/bind/named.conf.local** di `Werkudara`.
```
zone "baratayuda.abimanyu.b21.com" {
    type master;
    file "/etc/bind/delegasi/baratayuda.abimanyu.b21.com";
};
```

![confbaratayuda](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/d5bca4fa-c0e3-4232-99d7-e6c4d133c2db)

Buat folder baru, yaitu **delegasi** pada **/etc/bind**.
```
mkdir /etc/bind/delegasi
```

*Copy file* **db.local** ke dalam folder **delegasi** dan ubah namanya menjadi [**baratayuda.abimanyu.b21.com**](baratayuda.abimanyu.b21.com).
```
cp /etc/bind/db.local /etc/bind/delegasi/baratayuda.abimanyu.b21.com
```

Buka *file* [**baratayuda.abimanyu.b21.com**](baratayuda.abimanyu.b21.com) dan edit seperti konfigurasi berikut.

![Screenshot 2023-10-17 171241](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/d19f2392-f4d6-4e47-9c0e-44d69661ec23)

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.baratayuda.abimanyu.b21.com**](www.baratayuda.abimanyu.b21.com) untuk membuat alias yang mengarahkan domain ke [**baratayuda.abimanyu.b21.com**](baratayuda.abimanyu.b21.com).

*Restart* **bind9**.
```
service bind9 restart
```

### Sadewa atau Nakula

Lakukan *testing* pada `Sadewa` dan `Nakula` untuk cek apakah [**baratayuda.abimanyu.b21.com**](baratayuda.abimanyu.b21.com) atau [**www.baratayuda.abimanyu.b21.com**](www.baratayuda.abimanyu.b21.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

![pingbar](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/ead20e88-dc8e-4a3b-908d-536c9ff405b9)

![pingwwwbar](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/0a703e50-f9ee-4627-a919-8332ee4b867e)

## Nomor 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui `Werkudara` dengan akses `rjp.baratayuda.abimanyu.yyy.com` dengan alias `www.rjp.baratayuda.abimanyu.yyy.com` yang mengarah ke `Abimanyu`.

## Jawaban

### Werkudara

Buka *file* [**baratayuda.abimanyu.b21.com**](baratayuda.abimanyu.b21.com) dan edit seperti konfigurasi berikut.

![rjp](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/fd73a3ed-d466-45a8-a95d-f9636b7f1642)

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.rjp.baratayuda.abimanyu.b21.com**](www.rjp.baratayuda.abimanyu.b21.com) untuk membuat alias yang mengarahkan domain ke [**baratayuda.abimanyu.b21.com**](baratayuda.abimanyu.b21.com).

*Restart* **bind9**.
```
service bind9 restart
```

### Sadewa atau Nakula

Lakukan *testing* pada `Sadewa` dan `Nakula` untuk cek apakah [**rjp.baratayuda.abimanyu.yyy.com**](rjp.baratayuda.abimanyu.yyy.com) atau [**www.rjp.baratayuda.abimanyu.yyy.com**](www.rjp.baratayuda.abimanyu.yyy.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

![pingrjp](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/5f6cd1f1-b8cc-4f51-88d2-af491981fa02)

![pingwwwrjp](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/9f63bf3e-67c1-41ef-a8b3-e1216dbbfd1f)

## Nomor 9
`Arjuna` merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu `Prabakusuma`, `Abimanyu`, dan `Wisanggeni`. Lakukan deployment pada masing-masing worker.

## Jawaban

### Prabakusuma, Abimanyu, dan Wisanggeni

Melakukan instalasi dan setup **nginx dan php** terlebih dahulu pada `Prabakusuma`, `Abimanyu`, dan `Wisanggeni` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
 apt-get update && apt install nginx php php-fpm -y
```

*Start* **nginx**.
```
service nginx start
```

### Sadewa dan Nakula

Melakukan instalasi **lynx** terlebih dahulu pada `Sadewa` dan `Nakula` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install lynx -y
```

Lalu, *testing* apakah sudah terinstall dengan benar dengan `lynx` ke [**google.com**](google.com). Sebagai contoh pada `Sadewa`:

![google](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/5cfb9bed-eca3-4033-8582-0c253797340e)

## Nomor 10
Kemudian gunakan algoritma `Round Robin` untuk Load Balancer pada `Arjuna`. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003
    
## Jawaban

Sebelum mengerjakan perlu untuk melakukan setup terlebih dahulu. Karena telah berhasil melakukan deployment pada nomor 9. Hanya perlu mengubah masing-masing port pada worker menuju port yang telah ditentukan yaitu `Prabakusuma:8001, Abimanyu:8002, Wisanggeni:8003`. Kita juga perlu mengubah port `load-balancing` dengan menambahkan `:800X` pada masing-masing server

### Script

**Arjuna (Load Balancing)**
```
upstream backend {
  server 192.173.3.2:8001; # IP PrabuKusuma
  server 192.173.3.3:8002; # IP Abimanyu
  server 192.173.3.4:8003; # IP Wisanggeni
}
```

**PrabuKusuma, Abimanyu, Wisanggeni**

X adalah port yang telah ditentukan sesuai `worker` masing-masing
```
echo 'server {
        listen 800X;

        root /var/www/jarkom;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}' > /etc/nginx/sites-available/jarkom
```
### Result

![Screenshot 2023-10-17 180547](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/2bae173f-75ee-4be7-b9e8-6a90e7b9c179)

![lynxabi](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/3041c579-a87c-450b-af93-40e470cb7b03)

![Screenshot 2023-10-17 180556](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/83782366-7ebf-4c92-a1db-53a08458dc9c)

![lynxpraba](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/d8940d38-10eb-4ae1-a88b-7c29e188ec54)

![Screenshot 2023-10-17 180602](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/d9d37f75-a1b0-4f84-883c-c20a0a407d45)

![lynxwisang](https://github.com/LatomTrust/Jarkom-Modul-2-B21-2023/assets/114276069/a5fbfeb2-be89-4450-9892-e4772522f07b)

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


