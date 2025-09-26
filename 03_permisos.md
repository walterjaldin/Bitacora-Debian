# Capítulo 3 — Permisos y Propiedades

## Modelo de permisos
- **Clases**: `u` (user), `g` (group), `o` (others), `a` (all).
- **Operadores**: `+` añade, `-` quita, `=` asigna exacto.
- **Bits especiales**: `suid(4)`, `sgid(2)`, `sticky(1)` → octal: ej. `1755`.

## `chmod`
- **Simbólico**: `chmod u+rwx,g+rx,o-r archivo`
- **Octal**: `chmod 755 script.sh`
- **Recursivo**: `-R`
- `--reference=<archivo>` copiar permisos.
```bash
chmod -R u=rwX,go=rX ./public
```

## `chown`
Cambia propietario/grupo.
- `usuario:grupo` ó `:grupo` solo grupo, `-R` recursivo, `--from` condicional.
```bash
sudo chown -R www-data:www-data /var/www/app
```

## `chgrp`
Cambia grupo.
```bash
sudo chgrp -R developers repo/
```

## `umask`
Máscara de creación (se resta). Común: `022` (archivos 644, dirs 755).
```bash
umask 027
```

## Permisos avanzados
- **suid** en binarios que requieren privilegios: `chmod u+s /usr/bin/ping`
- **sgid** en dirs para heredar grupo: `chmod g+s /srv/compartido`
- **sticky** en /tmp: evita borrar ajenos: `chmod +t /tmp`
