# 🧱 Bitácora de Comandos – Fase 3: Hardening FTP + Hosting Multi-Sitio Apache

---

## 🌐 Área 1: Configuración de Red e Interfaces

### 🧬 Debian `/etc/network/interfaces`
```bash
sudo nano /etc/network/interfaces
```
**Configuración aplicada:**
```bash
# Interfaz principal
auto ens34
iface ens34 inet static
  address 172.20.10.12
  netmask 255.255.255.0
  dns-nameservers 8.8.8.8 200.87.100.10

# Segunda IP virtual
auto ens34:1
iface ens34:1 inet static
  address 172.20.10.13
  netmask 255.255.255.0

# Tercera IP virtual
auto ens34:2
iface ens34:2 inet static
  address 172.20.10.11
  netmask 255.255.255.0
```

### 🧬 Ubuntu `/etc/netplan/01-netcfg.yaml`
```bash
sudo nano /etc/netplan/01-netcfg.yaml
```
**Configuración aplicada:**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens33:
      addresses:
        - 172.20.10.14/24
        - 172.20.10.15/24
        - 172.20.10.16/24
      nameservers:
        addresses: [8.8.8.8, 200.87.100.10]
```
**Aplicar cambios:**
```bash
sudo netplan apply
ip a show ens33
```

---

## 👥 Área 2: Creación de Usuarios, Grupos y Estructura FTP

### 🔹 Crear grupo y usuarios
```bash
sudo groupadd webmasters

sudo useradd -m -d /srv/ftp/websites/rootwebservers/website3 \
  -s /usr/local/bin/ghost_silent -g webmasters superwebmaster

sudo useradd -m -d /srv/ftp/websites/rootwebservers/website1 \
  -s /usr/local/bin/ghost_silent -g webmasters webmaster1

sudo useradd -m -d /srv/ftp/websites/rootwebservers/website2 \
  -s /usr/local/bin/ghost_silent -g webmasters webmaster2

sudo passwd superwebmaster
sudo passwd webmaster1
sudo passwd webmaster2
```

### 🔹 Crear estructura de directorios
```bash
sudo mkdir -p /srv/ftp/websites/rootwebservers/{website1,website2,website3}/public_html
```

### 🔹 Asignar permisos y grupos
```bash
sudo chown -R superwebmaster:webmasters /srv/ftp/websites/rootwebservers
sudo chmod -R 775 /srv/ftp/websites/rootwebservers
sudo chmod o+rx /srv /srv/ftp /srv/ftp/websites /srv/ftp/websites/rootwebservers
```

### 🔹 Verificar estructura
```bash
ls -l /srv/ftp/websites/rootwebservers/
```

---

## 🔐 Área 3: Configuración del Servidor FTP (vsftpd)

### 🔹 Instalar vsftpd
```bash
sudo apt install vsftpd -y
```

### 🔹 Editar configuración principal
```bash
sudo nano /etc/vsftpd.conf
```

**Configuración aplicada:**
```bash
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
chroot_local_user=YES
allow_writeable_chroot=YES
user_config_dir=/etc/vsftpd_user_conf
xferlog_enable=YES
xferlog_std_format=YES
xferlog_file=/var/log/vsftpd.log
use_localtime=YES
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
ftpd_banner=Bienvenido al servidor FTP multi-sitio administrado por Superwebmaster
userlist_enable=YES
userlist_deny=NO
userlist_file=/etc/vsftpd.userlist
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
```

### 🔹 Crear configuraciones personalizadas por usuario
```bash
sudo mkdir /etc/vsftpd_user_conf
```

#### `/etc/vsftpd_user_conf/webmaster1`
```
local_root=/srv/ftp/websites/rootwebservers/website1/public_html
```

#### `/etc/vsftpd_user_conf/webmaster2`
```
local_root=/srv/ftp/websites/rootwebservers/website2/public_html
```

#### `/etc/vsftpd_user_conf/superwebmaster`
```
local_root=/srv/ftp/websites/rootwebservers/website3/public_html
```

### 🔹 Lista blanca de usuarios FTP
```bash
sudo nano /etc/vsftpd.userlist
```
**Contenido:**
```
superwebmaster
webmaster1
webmaster2
```

### 🔹 Reiniciar servicio FTP
```bash
sudo systemctl restart vsftpd
sudo systemctl status vsftpd
```

---

## 🌍 Área 4: Configuración de Apache2 y VirtualHosts

### 🔹 Instalar Apache2
```bash
sudo apt install apache2 -y
```

### 🔹 Crear enlaces simbólicos entre FTP y Apache
```bash
sudo ln -s /srv/ftp/websites/rootwebservers/website1/public_html /var/www/website1
sudo ln -s /srv/ftp/websites/rootwebservers/website2/public_html /var/www/website2
sudo ln -s /srv/ftp/websites/rootwebservers/website3/public_html /var/www/website3
```

### 🔹 Crear VirtualHosts

#### `/etc/apache2/sites-available/website1.conf`
```apache
<VirtualHost 172.20.10.12:80>
    ServerAdmin webmaster1@localhost
    ServerName www.website1.debian
    DocumentRoot /var/www/website1
    ErrorLog ${APACHE_LOG_DIR}/website1_error.log
    CustomLog ${APACHE_LOG_DIR}/website1_access.log combined
</VirtualHost>
```

#### `/etc/apache2/sites-available/website2.conf`
```apache
<VirtualHost 172.20.10.13:80>
    ServerAdmin webmaster2@localhost
    ServerName www.website2.debian
    DocumentRoot /var/www/website2
    ErrorLog ${APACHE_LOG_DIR}/website2_error.log
    CustomLog ${APACHE_LOG_DIR}/website2_access.log combined
</VirtualHost>
```

#### `/etc/apache2/sites-available/website3.conf`
```apache
<VirtualHost 172.20.10.11:80>
    ServerAdmin superwebmaster@localhost
    ServerName www.website3.debian
    DocumentRoot /var/www/website3
    ErrorLog ${APACHE_LOG_DIR}/website3_error.log
    CustomLog ${APACHE_LOG_DIR}/website3_access.log combined
</VirtualHost>
```

### 🔹 Habilitar los sitios
```bash
sudo a2ensite website1.conf
sudo a2ensite website2.conf
sudo a2ensite website3.conf
sudo systemctl reload apache2
```

### 🔹 Verificar sitios activos
```bash
sudo apache2ctl -S
```

---

## 🗂️ Área 5: Configuración del archivo hosts (Cliente)

### En Windows
Ruta:
```
C:\Windows\System32\drivers\etc\hosts
```

### En Linux
Ruta:
```
/etc/hosts
```

**Entradas agregadas:**
```
# Servidor Debian
172.20.10.11   www.website3.debian
172.20.10.12   www.website1.debian
172.20.10.13   www.website2.debian

# Servidor Ubuntu
172.20.10.14   www.website1.ubuntu
172.20.10.15   www.website2.ubuntu
172.20.10.16   www.website3.ubuntu
```

---

## 🧪 Área 6: Verificación de Servicios y Conectividad

### 🔹 Verificar acceso web
```bash
curl http://172.20.10.12
curl http://www.website1.debian
```

### 🔹 Verificar FTP (CLI)
```bash
ftp 172.20.10.11
Name: superwebmaster
Password: *****
ftp> ls
ftp> put index.html
```

### 🔹 Verificar desde FileZilla
- **Host:** IP o nombre del sitio  
- **Usuario:** webmaster1 / webmaster2 / superwebmaster  
- **Puerto:** 21  
- **Modo:** FTP (sin TLS)

---

## 🛠️ Área 7: Permisos, mantenimiento y monitoreo

### 🔹 Verificar permisos
```bash
ls -ld /srv/ftp/websites/rootwebservers/website*/public_html
```

### 🔹 Verificar logs FTP
```bash
sudo tail -f /var/log/vsftpd.log
```

### 🔹 Reinicio de servicios generales
```bash
sudo systemctl restart apache2
sudo systemctl restart vsftpd
```

