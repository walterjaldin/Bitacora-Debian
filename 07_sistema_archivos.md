# Capítulo 7 — Sistema de Archivos

## `df`
Uso de disco por sistema de archivos.
- `-h` legible, `-T` tipo, `--total` total.
```bash
df -hT
```

## `du`
Uso por directorio/archivo.
- `-h` legible, `-s` resumen, `--max-depth=N` profundidad.
```bash
du -h --max-depth=1 /var | sort -h
```

## `lsblk`, `blkid`
Listar bloques y UUIDs/FS.
```bash
lsblk -f
sudo blkid
```

## `mount` / `umount`
Montaje manual y desmontaje.
- `-t` tipo, `-o` opciones (`rw,ro,noexec,nosuid,nodev,uid=,gid=,umask=`).
```bash
sudo mount -t ext4 /dev/sdb1 /mnt/data -o noatime
sudo umount /mnt/data
```

## `/etc/fstab`
Montajes persistentes (UUID recomendado).
```
UUID=<uuid>  /mnt/data  ext4  defaults,noatime  0  2
```

## `file`, `stat`
Tipo de archivo y metadatos detallados.
```bash
file bin/app
stat bin/app
```

## Particionado/LVM (mención)
- `fdisk`, `parted` (MBR/GPT), `pvcreate`, `vgcreate`, `lvcreate`, `mkfs.ext4`, `tune2fs`, `e2fsck`.
