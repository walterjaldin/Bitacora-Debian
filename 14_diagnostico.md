# Capítulo 14 — Diagnóstico y Monitoreo

## Recursos del sistema
- `uptime`, `hostnamectl`, `lsb_release -a` (paquete `lsb-release`), `neofetch` (tercero).

## Memoria/CPU
```bash
free -h
vmstat 5
top
```
- `mpstat -P ALL 5` (sysstat), `iostat -xz 5` (sysstat).

## Discos/IO
```bash
dmesg -T | tail -100
sudo smartctl -a /dev/sda   # paquete smartmontools
sudo hdparm -Tt /dev/sda
```

## Red
```bash
ip -br a
ss -s
mtr -rw 8.8.8.8   # paquete mtr
nc -zv host 80    # netcat
```

## Archivos abiertos
```bash
sudo lsof -iTCP -sTCP:LISTEN -n -P | column -t
```

## Espacio
```bash
du -h --max-depth=1 /var | sort -h
ncdu /var   # paquete ncdu
```

## Logs rápidos
```bash
journalctl -p err -n 100 --no-pager
grep -Rin "ERROR" /var/log/
```
