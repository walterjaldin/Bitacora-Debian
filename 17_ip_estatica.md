
# Capítulo 17 — Configuración de IP Estática en Debian

En Debian, la configuración de red se puede realizar mediante **interfaces clásicas**, **Netplan** (en derivados como Ubuntu) o con **NetworkManager**. Debian por defecto utiliza `/etc/network/interfaces` o el servicio `systemd-networkd` en versiones recientes.

---

## 1. Ver interfaces de red disponibles
```bash
ip addr show
ip -br a
```

Ejemplo de salida:
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    inet 192.168.1.20/24 brd 192.168.1.255 scope global dynamic enp0s3
```

---

## 2. Configuración con `/etc/network/interfaces`
Archivo: `/etc/network/interfaces`

Ejemplo de IP estática:
```
auto enp0s3
iface enp0s3 inet static
    address 192.168.1.50
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 1.1.1.1
```

Reiniciar la red:
```bash
sudo systemctl restart networking
```

Verificar:
```bash
ip addr show enp0s3
ping -c 4 8.8.8.8
```

---

## 3. Configuración con `systemd-networkd`
Archivos en: `/etc/systemd/network/`

Ejemplo: `/etc/systemd/network/10-static.network`
```
[Match]
Name=enp0s3

[Network]
Address=192.168.1.50/24
Gateway=192.168.1.1
DNS=8.8.8.8
DNS=1.1.1.1
```

Aplicar cambios:
```bash
sudo systemctl enable systemd-networkd --now
sudo systemctl restart systemd-networkd
```

---

## 4. Configuración temporal con `ip`
Configurar IP manualmente (se pierde al reiniciar):
```bash
sudo ip addr flush dev enp0s3
sudo ip addr add 192.168.1.50/24 dev enp0s3
sudo ip route add default via 192.168.1.1
```

Verificar DNS: editar `/etc/resolv.conf`
```
nameserver 8.8.8.8
nameserver 1.1.1.1
```

---

## 5. Usando `nmcli` (si se usa NetworkManager)
```bash
nmcli con add type ethernet ifname enp0s3 con-name static1     ip4 192.168.1.50/24 gw4 192.168.1.1
nmcli con mod static1 ipv4.dns "8.8.8.8 1.1.1.1"
nmcli con up static1
```

---

## 6. Verificación final
- Comprobar IP:
```bash
ip -4 addr show enp0s3
```
- Comprobar puerta de enlace:
```bash
ip route show
```
- Comprobar conectividad:
```bash
ping -c 4 google.com
```
