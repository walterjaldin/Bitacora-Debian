# Capítulo 13 — Scripts y Automatización (Bash)

## Estructura básica
```bash
#!/usr/bin/env bash
set -Eeuo pipefail
IFS=$'\n\t'
```

## Variables, arrays y mapas
```bash
NOMBRE="Walter"
ARR=(uno dos tres)
declare -A MAPA=([clave]=valor)
echo "${ARR[1]} ${MAPA[clave]}"
```

## Parámetros y parsing
```bash
while getopts "u:p:h" opt; do
  case "$opt" in
    u) USERNAME="$OPTARG" ;;
    p) PORT="$OPTARG" ;;
    h) echo "Uso: $0 -u user -p port"; exit 0 ;;
  esac
done
```

## Condicionales y bucles
```bash
if [[ -f /etc/debian_version ]]; then echo "Debian"; fi
for f in *.log; do gzip -9 "$f"; done
```

## Funciones, trap y logging
```bash
log(){ printf "[%s] %s\n" "$(date +%F\ %T)" "$*"; }
trap 'log "Error en línea $LINENO"; exit 1' ERR
```

## Cron
```bash
crontab -e
# Ej: todos los días 03:00
0 3 * * * /usr/local/bin/backup.sh
```
