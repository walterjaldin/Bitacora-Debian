# Capítulo 12 — Seguridad Básica

## `sudo` y `visudo`
- Añadir usuario a sudo: `sudo usermod -aG sudo usuario`
- Editar sudoers de forma segura: `sudo visudo` (en `/etc/sudoers` o `/etc/sudoers.d/`)
- Ejemplo (NOPASSWD para grupo admin en comando específico):
```
%admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

## Permisos sensibles
- Revisar permisos de claves: `chmod 600 ~/.ssh/id_rsa`
- Directorio `.ssh`: `chmod 700 ~/.ssh`

## Inicios de sesión
- `who`, `w`, `last`, `lastlog`
```bash
last -a | head
```

## SSH (menciones)
- Servicio: `sudo systemctl status ssh`
- Config: `/etc/ssh/sshd_config` (cambios requieren `systemctl reload ssh`)
- Claves: `ssh-keygen -t ed25519 -C "comentario"`

## Firewall (ufw opcional)
```bash
sudo apt install ufw
sudo ufw allow OpenSSH
sudo ufw allow 80,443/tcp
sudo ufw enable
sudo ufw status verbose
```
