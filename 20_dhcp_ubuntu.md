# 🧾 Bitácora de Comandos – Servidor DHCP en Ubuntu

**Sistema Operativo:** Ubuntu Server 22.04 LTS  
**Servicio:** DHCP (Dynamic Host Configuration Protocol)  
**Autor:** Walter Jaldin Gonzales  
**Objetivo:** Implementar un servidor DHCP para asignar direcciones IP automáticas a clientes dentro de una red local.

---

## 🧩 1. Instalación del Servidor DHCP

```bash
sudo apt update
sudo apt install isc-dhcp-server -y
```

---

## ⚙️ 2. Configuración de la Interfaz de Red

Editar el archivo `/etc/default/isc-dhcp-server`:

```bash
sudo nano /etc/default/isc-dhcp-server
```

Buscar la línea:

```bash
INTERFACESv4=""
```

Y modificarla para indicar la interfaz que entregará direcciones IP (por ejemplo `ens33`):

```bash
INTERFACESv4="ens33"
```

> Puedes verificar tus interfaces disponibles con:
> ```bash
> ip a
> ```

---

## 🧠 3. Configuración Principal del DHCP

Abrir el archivo principal de configuración:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Ejemplo de configuración general y red local:

```bash
# Configuración general del servidor DHCP
default-lease-time 600;
max-lease-time 7200;
authoritative;

# Declaración de la subred
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
  option subnet-mask 255.255.255.0;
  option broadcast-address 192.168.10.255;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  option domain-name "redlocal.local";
}
```

---

## 📡 4. Asignación de IP Fija (Reserva)

Para asignar una IP fija a un cliente específico (por ejemplo un servidor o impresora), agregar al final del archivo:

```bash
host servidor-web {
  hardware ethernet 08:00:27:ab:cd:ef;
  fixed-address 192.168.10.50;
}
```

> Reemplaza la dirección MAC (`08:00:27:ab:cd:ef`) por la de tu cliente.  
> Puedes verla con:  
> ```bash
> ip link show
> ```

---

## 🔁 5. Reiniciar y Habilitar el Servicio DHCP

```bash
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
sudo systemctl status isc-dhcp-server
```

---

## 🧪 6. Verificación del Servicio

Verificar que el servicio esté activo:

```bash
sudo systemctl status isc-dhcp-server
```

Ver los logs para confirmar asignaciones de IP:

```bash
sudo tail -f /var/log/syslog | grep DHCP
```

---

## 💻 7. Prueba en un Cliente

En otro equipo de la misma red:

1. Configura la interfaz como **Automática (DHCP)**.
2. Verifica la IP obtenida:
   ```bash
   ip a
   ```
3. Prueba conectividad con el gateway:
   ```bash
   ping 192.168.10.1
   ```

---

## 🧰 8. Comandos de Mantenimiento

| Acción | Comando |
|--------|----------|
| Reiniciar el servicio | `sudo systemctl restart isc-dhcp-server` |
| Ver estado del servicio | `sudo systemctl status isc-dhcp-server` |
| Ver leases activos | `cat /var/lib/dhcp/dhcpd.leases` |
| Recargar configuración | `sudo systemctl reload isc-dhcp-server` |
| Habilitar inicio automático | `sudo systemctl enable isc-dhcp-server` |
| Ver logs DHCP en tiempo real | `sudo journalctl -u isc-dhcp-server -f` |

---

## 🧱 9. Configuración Adicional (Opcional)

### 🔸 DHCP para múltiples subredes
Si el servidor debe atender más de una red:

```bash
subnet 192.168.10.0 netmask 255.255.255.0 {
  range 192.168.10.100 192.168.10.200;
  option routers 192.168.10.1;
}

subnet 192.168.20.0 netmask 255.255.255.0 {
  range 192.168.20.100 192.168.20.200;
  option routers 192.168.20.1;
}
```

### 🔸 Servidor DHCP con Puerta de Enlace a Internet
Si el router de salida es diferente al servidor DHCP:

```bash
option routers 192.168.10.254;
```

---

## ✅ 10. Validación Final

Verifica que los clientes reciban IP dentro del rango especificado y puedan acceder a Internet o la red local.  
Usa los siguientes comandos para diagnóstico:

```bash
sudo dhcp-lease-list --lease
sudo systemctl status isc-dhcp-server
sudo netstat -tulpn | grep dhcp
```

---

📘 **Conclusión:**  
Con esta configuración, el servidor DHCP en Ubuntu asignará automáticamente direcciones IP, gateway y DNS a todos los dispositivos de la red definida, optimizando la administración de red local.

---

**Autor:** Walter Jaldin Gonzales  
**Materia:** Redes y Servicios – Implementación de Servidor DHCP  
**Fecha:** 2025
