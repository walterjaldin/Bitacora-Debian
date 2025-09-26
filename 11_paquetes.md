# Capítulo 11 — Paquetes en Debian (APT/DPKG)

## `apt`
- Índices: `apt update`
- Actualizar: `apt upgrade`, `apt full-upgrade` (puede remover/instalar)
- Instalar: `apt install pkg1 pkg2`
- Eliminar: `apt remove pkg`, `apt purge pkg` (incluye config)
- Limpieza: `apt autoremove`, `apt clean`
- Consulta: `apt search`, `apt show`, `apt list --installed | --upgradable`
- Políticas: `apt policy paquete`
- Flags: `-y` asume sí, `-s` simulación, `--no-install-recommends`.
```bash
sudo apt update && sudo apt install --no-install-recommends nginx
```

## `dpkg`
- Instalar local: `dpkg -i paquete.deb`
- Quitar: `dpkg -r paquete`, `dpkg -P paquete` (purge)
- Listar instalados: `dpkg -l | grep nginx`
- Archivos de un paquete: `dpkg -L paquete`
- ¿Qué paquete provee archivo?: `dpkg -S /ruta/archivo`
- Info dentro de .deb: `dpkg -I paquete.deb`
```bash
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt -f install  # reparar dependencias
```
