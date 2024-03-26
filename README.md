# Sistem Terdisribusi

## Konfigurasi Web-Server sister.local
- Instal Ubuntu 22.04.3 LTS pada Microsoft Store

- Mengganti source list Ubuntu
```
sudo nano /etc/apt/sources.list
```
menjadi
```
deb http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse

deb http://archive.canonical.com/ubuntu/ jammy partner
deb-src http://archive.canonical.com/ubuntu/ jammy partner
```
sudo apt update; sudo apt upgrade -y

- Install Nginx
```
sudo apt intall nginx nginx-extras
```
- Masuk ke Direktori Nginx
```
cd /etc/nginx/sites-enabled/
```
```
ls -la
```
![image](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/059c81be-2ad4-4107-991e-d4a078221992)

- Buat dan konfigurasi file sister.local
```
sudo cp default sister.local
```
```
sudo nano default sister.local
```
```
server {
        listen 80;
        listen [::]:80;

        server_name sister.local;

        root /var/www/html;
        index index.html

        location / {
                  try_files $uri $uri/ =404;
        }
}
```
- Coba lakukan tes Nginx
```
sudo nginx -t
```
- Jika berhasil
![image](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/0468ff22-bd85-4239-a87f-4cb07400cd61)
- Jika gagal
![image](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/9255bb1f-8a62-4e07-9938-a2f9ebf8753b)

- Setelah berhasil reload Nginx
```
sudo nginx -s reload
```

- Masuk ke direktori html
```
cd /var/www/html/
```

- Cari tau isi direktori html
```
ls
```
![image](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/1a3da6c2-4374-43bd-bcf6-3639c54fb0b4)

- Copy isi file nginx.html ke index.html
```
sudo cp index.nginx-debian.html index.html
```

- Edit konfigurasi pada index.html
```
sudo nano index.html
```
![image](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/528a0bc3-99be-4a0b-ac21-3ec9330531ed)

- Buka Notepad as Admin, kemudian open file hosts pada direktori berikut (Tipe file: All files)
```
C:\Windows\System32\drivers\etc
```
Paling bawah berikan 127.0.0.1        sister.local
![image](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/42d425b9-b25b-47c3-bb63-07b5479694d1)

- Coba buka browser kemudian search sister.local
![hasil1](https://github.com/marieroseoo/Sistem-Terdisribusi-Mencoba-LXC/assets/150213177/c0f9ee78-d9e1-4b72-8058-dcaee5db4206)

