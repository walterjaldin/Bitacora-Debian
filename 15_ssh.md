
# Capítulo 15 — SSH (Secure Shell) en Debian

SSH (Secure Shell) es un protocolo para administrar sistemas y transferir archivos de forma segura a través de la red.

---

## 1. Conexión básica con `ssh`
```bash
ssh usuario@host
ssh -p 2222 usuario@host   # puerto personalizado
ssh -i ~/.ssh/id_rsa usuario@host   # clave privada
```

### Opciones útiles
- `-v`, `-vv`, `-vvv` → modo debug detallado.
- `-C` → comprimir.
- `-X` → habilitar reenvío X11.
- `-L` → tunel local (`ssh -L 8080:localhost:80 usuario@host`).
- `-R` → tunel remoto (`ssh -R 9000:localhost:22 usuario@host`).
- `-D` → proxy dinámico (SOCKS5).

---

## 2. Archivos de configuración
### Cliente
- `~/.ssh/config`
```bash
Host servidor1
    HostName 192.168.1.10
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

### Servidor
- `/etc/ssh/sshd_config`
Parámetros comunes:
- `Port 22`
- `PermitRootLogin no`
- `PasswordAuthentication no`
- `PubkeyAuthentication yes`
- `AllowUsers user1 user2`
- `AllowGroups sshusers`

Reiniciar servicio tras cambios:
```bash
sudo systemctl restart ssh
```

---

## 3. Gestión de claves SSH
### Generar claves
```bash
ssh-keygen -t ed25519 -C "comentario"
```
Otras opciones: `-t rsa -b 4096`, `-f ruta`, `-N ""` sin passphrase.

### Copiar clave al servidor
```bash
ssh-copy-id usuario@host
```
(O manual: agregar a `~/.ssh/authorized_keys`).

### Permisos correctos
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

---

## 4. Transferencia de archivos
### `scp`
```bash
scp archivo.txt usuario@host:/destino/
scp -P 2222 -r carpeta usuario@host:/var/www/
```

### `sftp`
```bash
sftp usuario@host
sftp> put archivo.txt
sftp> get archivo.txt
sftp> mget *.log
sftp> mput *.conf
```

### `rsync` sobre SSH
```bash
rsync -avz -e "ssh -p 2222" ./local/ usuario@host:/destino/
```

---

## 5. Reenvío y túneles
### Túnel local
```bash
ssh -L 8080:localhost:80 usuario@host
```
Acceso local a un servicio remoto.

### Túnel remoto
```bash
ssh -R 9000:localhost:22 usuario@host
```

### Proxy dinámico (SOCKS5)
```bash
ssh -D 1080 usuario@host
```

---

## 6. Seguridad avanzada
- Desactivar login root (`PermitRootLogin no`).
- Usar claves en lugar de contraseñas (`PasswordAuthentication no`).
- Limitar usuarios/grupos (`AllowUsers`, `AllowGroups`).
- Usar `Fail2ban` o `ufw` para proteger.

---

## 7. Otros comandos útiles
### Probar conexión
```bash
ssh -v usuario@host
```

### Ejecutar un solo comando remoto
```bash
ssh usuario@host "uptime && df -h"
```

### Multiplexión de conexiones
```bash
Host servidor
    HostName 10.0.0.5
    User debian
    ControlMaster auto
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlPersist 10m
```

### Copiar archivo desde remoto a local con `tar` vía SSH
```bash
ssh usuario@host "tar czf - /var/www" | tar xzf - -C ./backup/
```

---

## 8. Logs de SSH
- Cliente: `~/.ssh/ssh.log` (si se redirige con `-E archivo.log`).
- Servidor: `/var/log/auth.log` en Debian.

