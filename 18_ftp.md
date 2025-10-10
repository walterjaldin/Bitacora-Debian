# 🧾 Bitácora de Comandos – Servicio FTP (vsftpd) en Debian

**Materia:** Administración de Servicios Linux  
**Tema:** Instalación, configuración y prueba del servidor FTP (vsftpd)  
**Distribución usada:** Debian 12 (Bookworm)  
**Autor:** Walter Jaldin Gonzales  
**Fecha:** 2025  

---

## 🧱 1. Instalación del servicio FTP

```bash
sudo apt update
sudo apt install vsftpd -y
```

Verificar que el servicio esté activo:
```bash
sudo systemctl status vsftpd
```

Si está detenido, iniciarlo:
```bash
sudo systemctl start vsftpd
```

Habilitar inicio automático:
```bash
sudo systemctl enable vsftpd
```

---

## ⚙️ 2. Archivo de configuración principal

Ruta principal del archivo:
```
/etc/vsftpd.conf
```

Antes de editar, hacer una copia de seguridad:
```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.backup
```

Editar el archivo:
```bash
sudo nano /etc/vsftpd.conf
```

Parámetros básicos recomendados:
```conf
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
local_umask=022
ftpd_banner=Bienvenido al servidor FTP de Debian
user_sub_token=$USER
local_root=/home/$USER/ftp
```

---

## 👤 3. Creación del usuario FTP

Crear un usuario para acceder al servidor:
```bash
sudo adduser ftpuser
```

Crear el directorio raíz del FTP:
```bash
sudo mkdir -p /home/ftpuser/ftp/upload
sudo chown nobody:nogroup /home/ftpuser/ftp
sudo chmod a-w /home/ftpuser/ftp
sudo chown ftpuser:ftpuser /home/ftpuser/ftp/upload
```

Reiniciar el servicio:
```bash
sudo systemctl restart vsftpd
```

---

## 🔐 4. Configuración de acceso (Firewall y puertos)

Permitir FTP en el firewall (si está activo ufw):
```bash
sudo ufw allow 20/tcp
sudo ufw allow 21/tcp
sudo ufw allow 990/tcp
sudo ufw allow 40000:50000/tcp
sudo ufw reload
```

Comprobar puertos:
```bash
sudo netstat -tulnp | grep ftp
```

---

## 🧪 5. Prueba de conexión local

Desde el mismo servidor:
```bash
ftp localhost
```

Desde otro cliente Linux:
```bash
ftp <IP_DEL_SERVIDOR>
```

Verificar acceso con usuario `ftpuser` y la contraseña establecida.

---

## 🌐 6. Prueba gráfica con FileZilla

1. Abrir **FileZilla**.
2. En "Host" colocar la IP del servidor.
3. Usuario: `ftpuser`
4. Contraseña: (la establecida)
5. Puerto: `21`
6. Hacer clic en **Conexión rápida**.

---

## 🧩 7. Opcional: FTP sobre TLS (FTPS)

Instalar paquete SSL:
```bash
sudo apt install openssl -y
```

Crear certificado:
```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
```

Editar `/etc/vsftpd.conf` y añadir:
```conf
rsa_cert_file=/etc/ssl/certs/vsftpd.crt
rsa_private_key_file=/etc/ssl/private/vsftpd.key
ssl_enable=YES
```

Reiniciar el servicio:
```bash
sudo systemctl restart vsftpd
```

---

## 🧹 8. Comandos útiles de mantenimiento

Ver logs del servicio:
```bash
sudo tail -f /var/log/vsftpd.log
```

Ver usuarios conectados:
```bash
sudo lsof -i :21
```

Desinstalar el servicio:
```bash
sudo apt remove vsftpd -y
```

---

## ✅ 9. Conclusiones

El servicio **vsftpd** es uno de los servidores FTP más seguros y ligeros disponibles en Linux.  
Su correcta configuración permite compartir archivos entre equipos dentro de una red local o externa de manera eficiente y segura.

---

✍️ **Autor:** *Walter Jaldin Gonzales*  
📅 **Fecha:** *10 de octubre de 2025*  
📄 **Archivo:** `18_ftp.md`
