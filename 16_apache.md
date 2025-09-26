
# Capítulo 16 — Apache HTTP Server en Debian

Apache es uno de los servidores web más utilizados en sistemas Linux. En Debian se instala con el paquete `apache2`.

---

## 1. Instalación y servicio
```bash
sudo apt update
sudo apt install apache2 -y
```

### Gestión del servicio
```bash
sudo systemctl status apache2
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl reload apache2
sudo systemctl enable apache2
```

Archivos binarios principales:
- `/usr/sbin/apache2` → ejecutable principal.
- `/usr/bin/apache2ctl` → utilidad para administrar.

---

## 2. Estructura de archivos en Debian
- `/etc/apache2/` → configuración principal.
    - `apache2.conf` → archivo principal de configuración.
    - `ports.conf` → puertos de escucha.
    - `sites-available/` → configuraciones de sitios disponibles.
    - `sites-enabled/` → enlaces simbólicos a los sitios activos.
    - `mods-available/` → módulos disponibles.
    - `mods-enabled/` → módulos habilitados.
    - `conf-available/` → configuraciones adicionales disponibles.
    - `conf-enabled/` → configuraciones adicionales habilitadas.
- `/var/www/` → directorios de contenido web (por defecto `/var/www/html`).
- `/var/log/apache2/` → logs de Apache.
    - `access.log` → registros de acceso.
    - `error.log` → registros de errores.
- `/lib/systemd/system/apache2.service` → archivo de unidad systemd.

---

## 3. Configuración básica
Archivo principal: `/etc/apache2/apache2.conf`

Parámetros comunes:
- `ServerRoot` → directorio raíz de Apache (por defecto `/etc/apache2`).
- `Listen` → puertos de escucha (definidos en `ports.conf`).
- `User` y `Group` → usuario y grupo que ejecutan Apache (por defecto `www-data`).
- `ServerName` → nombre del servidor (FQDN).
- `DocumentRoot` → ruta donde se encuentran los archivos web.

Ejemplo de `apache2.conf`:
```apache
ServerName www.ejemplo.com
ServerAdmin admin@ejemplo.com
DocumentRoot /var/www/html

<Directory /var/www/html>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

---

## 4. Virtual Hosts
### Crear un sitio nuevo
Archivo: `/etc/apache2/sites-available/misitio.conf`
```apache
<VirtualHost *:80>
    ServerName misitio.com
    ServerAlias www.misitio.com
    DocumentRoot /var/www/misitio
    ErrorLog ${APACHE_LOG_DIR}/misitio_error.log
    CustomLog ${APACHE_LOG_DIR}/misitio_access.log combined
</VirtualHost>
```

Activar el sitio:
```bash
sudo a2ensite misitio.conf
sudo systemctl reload apache2
```

Desactivar:
```bash
sudo a2dissite misitio.conf
```

---

## 5. Módulos de Apache
### Habilitar módulo
```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### Deshabilitar módulo
```bash
sudo a2dismod rewrite
```

Módulos comunes:
- `rewrite` → reescritura de URLs.
- `ssl` → soporte HTTPS.
- `headers` → control de cabeceras HTTP.
- `proxy` → proxy directo y reverso.
- `security2` → ModSecurity (WAF).

---

## 6. Archivos `.htaccess`
Permiten configurar opciones en un directorio específico (si está habilitado `AllowOverride`).

Ejemplo `.htaccess`:
```
Options -Indexes
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [QSA,L]
```

---

## 7. HTTPS y certificados
Instalar mod_ssl:
```bash
sudo apt install openssl ssl-cert
sudo a2enmod ssl
```

Ejemplo de VirtualHost con SSL (`/etc/apache2/sites-available/misitio-ssl.conf`):
```apache
<VirtualHost *:443>
    ServerName misitio.com
    DocumentRoot /var/www/misitio

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/misitio.crt
    SSLCertificateKeyFile /etc/ssl/private/misitio.key

    ErrorLog ${APACHE_LOG_DIR}/misitio_ssl_error.log
    CustomLog ${APACHE_LOG_DIR}/misitio_ssl_access.log combined
</VirtualHost>
```

Habilitar:
```bash
sudo a2ensite misitio-ssl.conf
sudo systemctl reload apache2
```

---

## 8. Logs y monitoreo
- Accesos: `/var/log/apache2/access.log`
- Errores: `/var/log/apache2/error.log`

Seguir logs en tiempo real:
```bash
tail -f /var/log/apache2/error.log
```

---

## 9. Seguridad recomendada
- Deshabilitar `Indexes` para evitar listado de directorios.
- Usar `ServerTokens Prod` y `ServerSignature Off` en `apache2.conf`.
- Limitar métodos HTTP permitidos:
```apache
<LimitExcept GET POST HEAD>
    Deny from all
</LimitExcept>
```
- Habilitar firewall (`ufw allow 'Apache Full'`).

