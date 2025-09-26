# Capítulo 9 — Red y Conectividad

## `ip` (reemplaza ifconfig/route)
- `ip addr` (`ip a`) ver IPs, `ip link` enlaces, `ip route` rutas.
- Añadir IP: `sudo ip addr add 192.168.1.50/24 dev eth0`
- Quitar IP: `sudo ip addr del 192.168.1.50/24 dev eth0`
- Ruta: `sudo ip route add 10.0.0.0/24 via 192.168.1.1`
- IPv6: `ip -6 addr`, `ip -6 route`.
```bash
ip -br a
```

## `ping`
- `-c N` conteo, `-i SEG` intervalo, `-w SEG` timeout total, `-4/-6` forzar protocolo.
```bash
ping -c 4 -i 0.2 8.8.8.8
```

## `traceroute` / `tracepath`
- `-n` sin resolver DNS, `-I` icmp, `-T` TCP, `-m` TTL máx.
```bash
sudo traceroute -n -I 1.1.1.1
```

## `ss` (socket statistics)
- `-t` TCP, `-u` UDP, `-l` listen, `-p` procesos, `-n` numérico, `-a` todo.
```bash
ss -tulnp
```

## `curl`
- `-I` HEAD, `-L` seguir redirecciones, `-O` guardar con nombre remoto, `-o` nombre,  
  `-d` datos (POST), `-H` header, `-X` método, `--retry N`, `-k` ignorar cert (no prod).
```bash
curl -L -O https://example.com/file.tar.gz
curl -H "Authorization: Bearer TOKEN" https://api.example.com/me
```

## `wget`
- `-c` continúa descarga, `-O` nombre, `--limit-rate`, `--no-check-certificate` (no prod).
```bash
wget -c -O iso.debian.iso https://cdimage.debian.org/debian.iso
```

## `scp` y `rsync`
- `scp -P PUERTO -r origen usuario@host:/destino`
- `rsync -avz --progress -e "ssh -p 2222" src/ user@host:/dst/`
```bash
rsync -avz --delete ./web/ debian@server:/var/www/html/
```
