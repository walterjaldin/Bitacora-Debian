# Capítulo 8 — Compresión y Empaquetado

## `tar`
- Crear: `tar -cvf archivo.tar dir/`
- Extraer: `tar -xvf archivo.tar -C destino/`
- Listar: `tar -tvf archivo.tar`
- Compresión: `-z` (gzip), `-j` (bzip2), `-J` (xz).
- Útiles: `--exclude`, `--strip-components=N`.
```bash
tar -czvf backup.tar.gz /etc --exclude=/etc/ssh
```

## `gzip` / `gunzip`
- `-k` mantener original, `-9` máxima compresión.
```bash
gzip -9k logs.txt
gunzip logs.txt.gz
```

## `xz` / `unxz`
- `-T0` hilos, `-9` compresión.
```bash
xz -T0 -9 imagen.iso
```

## `zip` / `unzip`
- `zip -r paquete.zip dir/`, `-9` máxima, `-x` excluir, `-P` contraseña (no recomendado para seguridad).
```bash
zip -r9 paquete.zip src/ -x "*.git*"
unzip -l paquete.zip
```
