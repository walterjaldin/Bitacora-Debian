# Capítulo 2 — Archivos y Directorios

## `touch`
Crea/actualiza archivos.
- `-a` solo atime, `-m` solo mtime, `-t [[CC]YY]MMDDhhmm[.ss]` fecha específica, `-r <ref>` copiar tiempos.
```bash
touch -t 202501011200.00 reporte.txt
```

## `cp`
Copia archivos.
- `-a` archivo + atributos (equivale a `-dR --preserve=all`), `-R` recursivo, `-r` recursivo (similar),  
  `-p` preserva modos/prop/tiempos, `-v` verbose, `-i` interactivo, `-u` solo si fuente es más nueva,  
  `-n` no sobrescribir, `--preserve=mode,ownership,timestamps,xattr,links,context`.
```bash
cp -a origen/ destino/
```

## `mv`
Mueve/renombra.
- `-i` preguntar, `-f` forzar, `-n` no sobrescribir, `-v` verbose.
```bash
mv -i archivo.txt docs/archivo.txt
```

## `rm`
Elimina.
- `-r` recursivo, `-f` forzar, `-d` directorio vacío, `-I` confirmar si >3 archivos, `-v` verbose, `--one-file-system`.
```bash
rm -rf build/ .cache/
```

## `mkdir`
Crea directorios.
- `-p` padres, `-m` modo, `-v` verbose.
```bash
mkdir -p -m 755 /opt/app/logs
```

## `rmdir`
Elimina directorios vacíos.
- `-p` eliminar padres vacíos, `-v` verbose.

## `install` (útil en despliegues)
Copia + permisos.
- `-D` crea dirs, `-m` modo, `-o` dueño, `-g` grupo.
```bash
install -Dm755 bin/app /usr/local/bin/app
```
