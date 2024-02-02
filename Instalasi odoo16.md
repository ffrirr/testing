# Installasi OdOO16
## 1. Persiapan
### Python  
cek versi python yang digunakan, direkomendasikan menggunakan python 3.9
```
$ python3 --version
```  
### PostgreSQL  
Install PostgreSQL dengan langkah-langkah berikut:  
```
# Create the file repository configuration:
sudo sh -c 'echo "deb https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
# Import the repository signing key:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
# Update the package lists:
sudo apt-get update
# Install the latest version of PostgreSQL.
# If you want a specific version, use 'postgresql-12' or similar instead of 'postgresql':
sudo apt-get -y install postgresql
```
Periksa versi PostgreSQL:  
```
postgres --version
```
Masuk ke PostgreSQL:
```
## sudo su postgres
psql
```
Buat user baru:
```
CREATE USER ‘odoo_user’ WITH PASSWORD ‘odoo_user’ CREATEDB CREATEROLE LOGIN;
```
Perintah ini akan membuat pengguna dengan nama 'odoo_user' dan kata sandi 'odoo_user', serta memberikan hak kepada mereka untuk membuat database, membuat peran, dan masuk ke server PostgreSQL.
Periksa apakah user berhasil dibuat:
```
\du
```
Keluar dari PostgreSQL:
```
\q
exit
```

### Git
Install Git:
```
sudo apt install git
```  
### Folder dan Repositori
Buat folder dan kloning repositori Odoo 16 dan training arkana:
```
mkdir -p /home/feri/odoo/{project,conf}
git clone https://github.com/odoo/odoo.git --branch 16.0 /home/feri/odoo/odoo
git clone https://github.com/nrkholid/arkana-training.git /home/feri/odoo/project/arkana-training
```
pada folder /odoo/project lakukan kloning repositori training arkana
```
git clone https://github.com/nrkholid/arkana-training.git
```
Salin file konfigurasi:
```
cp /home/feri/odoo/project/arkana-training/arkana-training.conf /home/feri/odoo/conf
```

mengkonfigurasi pada file /odoo/conf/arkana-training.conf
```
[options]
addons_path = /home/feri/odoo/odoo/addons,/home/feri/odoo/project/arkana-training
admin_passwd = odoo
db_host = localhost
db_name = false
db_password = 123Terra!
db_port = 5432
db_user = feri
dbfilter =
http_port = 8010
xmlrpc_port = 8010
list_db = True
log_db = False
log_db_level = warning
log_handler = :INFO
log_level = info
logfile =
default_productivity_apps = True
```

* [options]: Menunjukkan awal bagian konfigurasi.  
* addons_path: Menentukan direktori tempat Odoo mencari modul:
    /home/feri/odoo/odoo/addons (jalur default)  
    /home/feri/odoo/project/arkana-training (jalur modul kustom tambahan)  
* admin_passwd: Mengatur kata sandi untuk pengguna administratif(di sini, "odoo").

Koneksi Database:

* db_host: Nama host atau alamat IP server database (di sini, "localhost", yang berarti berada di mesin yang sama dengan Odoo).
* db_name: Nama database untuk terhubung (saat ini disetel ke "false", yang menunjukkan tidak ada database default).
* db_password: Kata sandi untuk pengguna database (di sini, "123Terra!").
* db_port: Nomor port server database (di sini, 5432, port PostgreSQL standar).
* db_user: Nama pengguna untuk menghubungkan ke database (di sini, "feri").
* dbfilter: Regex untuk memfilter database berdasarkan nama host atau subdomain (tidak disetel di sini).

Pengaturan Jaringan:

* http_port: Nomor port untuk permintaan HTTP (di sini, 8010).
* xmlrpc_port: Nomor port untuk permintaan XML-RPC (juga 8010 dalam konfigurasi ini).

Pengaturan Logging:

* list_db: Mengaktifkan daftar database yang tersedia di layar login (diatur ke True).
* log_db: Mencatat interaksi database (diatur ke False).
* log_db_level: Mengatur level logging untuk interaksi database (akan menjadi "warning" jika diaktifkan).
* log_handler: Menentukan custom logging handler (tidak disetel di sini).
* log_level: Level logging umum (diatur ke "info").
* logfile: Jalur ke file untuk output logging (tidak disetel, jadi log akan diarahkan ke konsol).

### virtual environment
Buat virtual environment:
```
python3 -m venv odoo16-env
```
Aktifkan virtual environment:
```
source odoo16-env/bin/activate
```
### Installasi Paket
Install paket wheel dan requirements.txt:
```
python3 -m pip install wheel
cd /home/feri/odoo/odoo
python3 -m pip install -r requirement.txt
```
## 2. Menjalankan odoo
```
python odoo16/odoo-bin -c conf/arkana-training.conf --dev xml
```
perintah ini akan menjalankan skrip Odoo versi 16 (odoo-bin) dengan menggunakan konfigurasi yang disebutkan dalam file arkana-trainingconf, dalam mode pengembangan (--dev), dan mungkin melakukanoperasi tertentu yang ditentukan oleh argumen xml.
lalu buka http://localhost:8010 di web browser dan masuklah kebasis data Odoo dengan akun administrator dasar: gunakan admin sebagai alamat email dan sebagai kata sandi.

## 3.Debuging Odoo  
Buka panel Debugger di Visual Studio Code (Ctrl + Shift + D), klik tombol gear untuk membuka konfigurasi launch.json. Tambahkan konfigurasi seperti berikut ini:
```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Arkana Training",
            "type": "python",
            "request": "launch",
            "stopOnEntry": false,
            "console": "integratedTerminal",
            "python": "/home/feri/odoo/odoo16-env/bin/python", // cari dengan which python3
            "program": "/home/feri/odoo/odoo/odoo-bin",
            "args": [
                "--config=/home/feri/odoo/conf/arkana-training.conf",
                "--dev=xml",
                // "--database=odoo16_arkana_training",
                // "--update=arkana_base_module”,
            ]
        },
    ]
}
```
Pastikan untuk mengganti path_to_odoo dengan path ke direktori Odoo di sistem Anda, dan path_to_config_file dengan path ke file konfigurasi Odoo (.conf). jalankan debugging dengan menekan f5

##4. Extra

### Git Branch:
Perintah ini digunakan untuk menampilkan daftar branch yang ada dalam repositori Git.
contoh:
```
git branch features/20240201_Feri_arkana_training main
```

### Git Pull:
Perintah ini digunakan untuk menarik (pull) perubahan dari remote repository ke branch lokal yang sedang aktif.

### Git Checkout:
Perintah ini memiliki dua fungsi utama. Pertama, untuk berpindah ke branch lain (dalam kasus ini, menentukan nama branch yang ingin Anda pindahkan). Kedua, untuk mengubah status file (misalnya, beralih ke branch lain dengan melewatkan nama branch).
contoh:
```
git checkout features/20240201_Feri_arkana_training
```
