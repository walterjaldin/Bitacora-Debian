# Capítulo 4 — Visualización y Búsqueda

## `cat`, `tac`, `nl`
Concatenar, invertir, numerar líneas.
```bash
nl -ba archivo.txt | less
```

## `less` / `more`
Paginadores.
- `less +F` seguir (tail -f), `/patrón` buscar, `n/N` siguiente/anterior.

## `head`, `tail`
- `head -n 50`, `tail -n 100`, `tail -f /var/log/syslog`.

## `grep`
Busca patrones.
- `-i` ignore case, `-r` recursivo, `-n` líneas, `-E` regex extendidas, `-F` texto fijo, `-v` invert match,
  `-w` palabra completa, `-x` línea completa, `-o` solo coincidencia, `-A/-B/-C` contexto, `--color=auto`.
```bash
grep -Rin --color=auto "ERROR" /var/log/
```

## `find`
Búsqueda por atributos.
- Por nombre: `-name "*.log"` (sensitivo), `-iname` (insensible).  
- Por tipo: `-type f|d|l`.  
- Por tamaño: `-size +100M` (>, `<` <).  
- Por tiempo: `-mtime +7` (días), `-mmin -30` (minutos).  
- Por permisos: `-perm 644`, `-perm -4000` (suid).  
- Acciones: `-delete`, `-print0 | xargs -0`, `-exec cmd {} +`.
```bash
find /var/www -type f -mtime +30 -name "*.log" -delete
```

## `which`, `whereis`, `type`
Ubicación y tipo de comando.
```bash
which nginx; whereis nginx; type cd
```

## `locate` (bd `updatedb`)
Búsqueda rápida en índice.
```bash
sudo updatedb && locate sshd_config
```
