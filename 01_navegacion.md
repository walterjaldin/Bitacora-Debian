# Capítulo 1 — Navegación

## `pwd`
Muestra el directorio actual.
- **Opciones**: `-L` (sigue enlaces simbólicos), `-P` (ruta física sin symlinks).
```bash
pwd -P
```

## `ls`
Lista archivos/directorios.
- **Opciones clave**:  
  `-l` largo, `-a` ocultos, `-h` tamaños legibles, `-R` recursivo, `-t` orden por tiempo,  
  `-S` ordenar por tamaño, `-r` inverso, `-1` una entrada por línea, `-F` sufijos tipo,  
  `-d` listar el directorio en sí, `-i` inode, `--color=auto` colores.
```bash
ls -lha --color=auto
```

## `cd`
Cambia de directorio.
- **Formas útiles**: `cd ..` subir, `cd -` volver, `cd ~` HOME, `cd ~/proyectos`.

## `tree` (paquete `tree`)
Árbol de directorios.
- `-L N` profundidad, `-a` ocultos, `-d` solo dirs, `-h` tamaños.
```bash
tree -L 2 -a
```

## `dirs`, `pushd`, `popd`
Pila de directorios.
```bash
pushd /etc && pushd /var/log && dirs && popd && popd
```
