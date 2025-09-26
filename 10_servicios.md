# Capítulo 10 — Servicios y Logs (systemd)

## `systemctl`
- Estado: `status`, `is-active`, `is-enabled`
- Control: `start`, `stop`, `restart`, `reload`
- Arranque: `enable`, `disable`
- Varios: `daemon-reload`, `list-unit-files`, `list-units`
```bash
sudo systemctl status ssh
sudo systemctl enable --now apache2
```

## Unidades comunes
- `*.service`, `*.socket`, `*.timer`, `*.target`.

## Edición/override
```bash
sudo systemctl edit nombre.service   # crea drop-in en /etc/systemd/system/nombre.service.d/
```

## `journalctl`
- `-u <service>` por servicio, `-f` seguimiento, `-n N` últimas N líneas,
  `--since "2025-09-01"`, `-p info|warn|err|crit`, `-k` mensajes del kernel, `-xe` detalle.
```bash
journalctl -u nginx -f
```

## Logs tradicionales
- `/var/log/syslog`, `/var/log/auth.log`, `/var/log/kern.log`, `/var/log/apache2/`, `/var/log/nginx/`.
