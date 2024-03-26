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

- Setelah berhasil reload Nginx
```
sudo nginx -s reload
```

- Masuk ke direktori html
```
cd /var/www/html/
```

- Copy isi file nginx.html ke index.html
```
sudo cp index.nginx-debian.html index.html
```

- Edit konfigurasi pada index.html
```
sudo nano index.html
```
![image](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/9f99b9b7-d846-416b-8b61-7eebddc6a5cb)

- Buka Notepad as Admin, kemudian open file hosts pada direktori berikut (Tipe file: All files). Paling bawah tambahkan 127.0.0.1 dan local.sister
```
C:\Windows\System32\drivers\etc
```
![image](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/24d5b317-e976-4f7a-821a-953594d9671d)

- Coba buka browser kemudian search sister.local
![hasil1](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/1cda38e6-711d-413e-812d-3292bd91dae2)

## Install Ubuntu 20 & 18 untuk IP Statis
- Install Ubuntu 20 (Focal) as microservice1
```
sudo lxc-create -n microservice1 -t download -- --dist ubuntu --release focal --arch amd64 --force-cache --server images.linuxcontainers.org
```
- Install Ubuntu 18 (Bionic) as microservice2
```
sudo lxc-create -n microservice2 -t download -- --dist ubuntu --release bionic --arch amd64 --force-cache --server images.linuxcontainers.org
```

- Cek status run microservice
```
sudo lxc-ls -f
```

- Run microservice
```
sudo lxc-start -n microservice1
sudo lxc-start -n microservice2
```

- Masuk ke microservice untuk konfigurasi ip statis
```
sudo lxc-attach -n microservice1
sudo lxc-attach -n microservice2
```

- Saat pertama kali konfigurasi setting new password
```
passwd
```

- Install nano sebelum konfig ip
```
apt install nano
```

- Cek IP
```
ip a
inet 10.0.3.198/24 brd 10.0.3.255 (microservice1 blog)
inet 10.0.3.175/24 brd 10.0.3.255 (microservice2 aboutus)
```

- Konfigurasi IP statis
```
nano /etc/netplan/10-lxc.yaml
```
![m1](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/0deef76f-4e54-4573-a1f4-fa3cb737b683)

- Restart
```
sudo netplan apply
```

- Tes ping google.com
```
ping google.com
```
![ping gugel](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/d81efc33-28d8-46b6-89bd-d26a303dd625)

- Setelah konfig lakukan update
```
apt update; apt upgrade -y
```

## Membuat Web-Server sister.local/blog & sister.local/aboutus
- Masuk salah satu microservice
- Instal Nginx
```
sudo apt install nginx nginx-extras
apt install curl
```

- Pindah ke direktori html & edit file nginx html
```
cd /var/www/html
nano index.nginx-debian.html
```
![blogedit](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/38546f49-5979-41a0-a38f-50117f0dce1e)
![aboutusedit](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/55a2c9b1-6e7f-4971-9b5a-a9ec905a5c2b)

- Cek sekilas apakah setelah semua file html yang di edit tidak kembali default
```
curl localhost
```

- Exit untuk keluar microservice
```
exit
```

- Masuk direktori parent
- Pindah ke direktori Nginx
```
cd /etc/nginx/sites-enabled/
```

- Edit sister.local
```
sudo nano sister.local
```
![proxyreversefix](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/eff9ded9-3784-4cc1-9870-4134857dc0a1)

- Cek konfigurasi
```
sudo nginx -t
```

- Reload konfigurasi
```
sudo nginx -s reload
```

- Masuk lagi ke dalam microservice untuk membuat mcsv1.local & mcsv2.local
```
sudo lxc-attach -n microservice1
sudo lxc-attach -n microservice2
```

- Pindah ke direktori Nginx, kemudian copy file default ke file mcsv
```
cd /etc/nginx/sites-enabled
cp default mcsv1.local
cp default mcsv2.local
```
- Edit isi file
```
nano mcsv1.local
nano mcsv2.local
```
![mcsv1](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/28a1835e-2298-46e8-bcd7-e7806799f4d5)
![mcsv2](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/c346ad30-9b28-4e8c-9723-c1b89c538b82)

- Cek konfigurasi
```
sudo nginx -t
```

- Reload konfigurasi
```
sudo nginx -s reload
```

- Masuk ke direktori html, kemudian kemudian copy file index nginx ke file index.html
```
cd /var/www/html
cp index.nginx-debian.html index.html
```

- Edit file hosts
```
sudo nano /etc/hosts
```
![hostsmcsv1](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/93e55b49-6269-4b1f-b067-2c8d9114a299)
![hostsmcsv2](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/64c8573f-0d1e-48f6-9626-d94e104713f9)

- Cek sekilas apakah sudah sesuai
```
curl mcsv1.local
curl mcsv2.local
```

- Kembali ke file parent
- Masuk kembali ke direktori nginx
```
cd /etc/nginx/sites-enabled
```

- Edit file hosts, tambahkan paling bawah
```
sudo nano /etc/hosts
10.0.3.198	mcsv1.local
10.0.3.175	mcsv2.local
```
![hostparent](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/6f524bc0-a820-487a-8ed0-af77dea71618)

- Cek konfigurasi
```
sudo nginx -t
```

- Reload konfigurasi
```
sudo nginx -s reload
```

- Coba pada browser
```
sister.local/blog
sister.local/aboutus
```
![hasil2](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/e4231b85-08ef-4b63-8c30-afb05108f197)
![hasil3](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/935f4c73-16dc-4d9d-9bc2-898dfe14ecb6)
