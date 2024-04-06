# Sistem Terdisribusi - Load Balancing Round Robin

## Membuat Linux Containers Microservice 3, 4, 5
- Install Debian 10 (buster) as Microservice 3, 4, 5
```
sudo lxc-create -n microservice3 -t download -- --dist debian --release buster --arch amd64 --force-cache --server images.linuxcontainers.org
sudo lxc-create -n microservice3 -t download -- --dist debian --release buster --arch amd64 --force-cache --server images.linuxcontainers.org
sudo lxc-create -n microservice5 -t download -- --dist debian --release buster --arch amd64 --force-cache --server images.linuxcontainers.org
```

- Start Microservice
```
sudo lxc-start -n microservice3
sudo lxc-start -n microservice4
sudo lxc-start -n microservice5
```

- Cek status run microservice (IP auto terisi)
```
sudo lxc-ls -f
```
![image](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/e1bdc365-8a79-4c04-a46a-ef20af1d4b56)


- IP yang didapat
```
10.0.3.113/24 brd 10.0.3.255
10.0.3.95/24 brd 10.0.3.255
10.0.3.233/24 brd 10.0.3.255
```

- Attach ke salah satu microservice untuk konfigurasi
```
sudo lxc-attach -n microservice3
sudo lxc-attach -n microservice4
sudo lxc-attach -n microservice5
```

- Setelah masuk salah satu microservice lakukan install beberapa tools (lakukan pada microservice lainnya)
```
apt update; apt upgrade -y
apt install nano
apt install curl
sudo apt install nginx nginx-extras
```

- Konfigurasi pada hosts tiap microservice
```
nano /etc/hosts
```
![etchosts3](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/cad73396-475f-4195-a059-65150033c39b)
![etchosts4](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/aa5871cf-dc5d-4e77-ae9b-eac089e05023)
![etchosts5](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/9d98ba4c-0e02-4f52-8fa3-cf31921e9e11)

- Edit file html untuk tampilan pada web browser
```
nano /var/www/html/index.nginx-debian.html
```
![html3](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/1a3cdedf-8c37-4599-84fc-c7d2ae1ced03)
![html4](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/a0f1aa30-9052-4f42-aa3d-4f07bb8d0064)
![html5](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/d2100ea8-ec9f-412e-8155-0b6c935e788a)


## Konfigurasi Hosts WSL
- Kembali ke parents

- Konfigurasi pada hosts parent
```
nano /etc/hosts
```
![etchostparent](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/b183443a-419c-4896-b4bc-6152eebfbe59)

- Konfigurasi pada sister.local
```
sudo nano /etc/nginx/sites-enabled/sister.local
```
![sisterlocal](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/a05abb42-9599-4fbc-ab6f-d15c59bf6830)


- Cek konfigurasi Nginx
```
sudo nginx -t
```

- Reload konfigurasi Nginx
```
sudo nginx -s reload
```

- Cek menggunakan curl apakah tampilan sudah sesuai pada terminal (ulangi command untuk output yang berbeda)
```
curl -i app.sister.local
```
![curl1](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/fb085539-38ee-40df-8837-c40154ecca5b)
![curl2](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/f9e4e54e-cddc-450f-b9dc-68ddc3da0cdd)
![curl3](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/01f01389-485b-4c79-a603-afbc89b5f897)

- Cek pada browser (NOTE: Untuk masuk ke server yang berbeda cukup lakukan reload pada tab browser)
```
app.sister.local
```
![hasil1](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/332e75e5-5813-4ac5-8128-f08788ef9a49)
![hasil2](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/011cb056-01de-4eff-bfc4-79fa6692940f)
![hasil3](https://github.com/marieroseoo/Sistem-Terdisribusi/assets/150213177/65217cbb-cce1-4531-8ef6-2f02df0d3400)
