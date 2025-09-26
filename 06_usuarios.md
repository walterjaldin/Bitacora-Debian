# Capítulo 6 — Usuarios y Grupos (Debian)

## `useradd` (bajo nivel)
- `-m` crear HOME, `-M` no crear HOME, `-d DIR` home personalizado, `-s SHELL`,
  `-c` comentario, `-u UID`, `-g GID` grupo primario, `-G` grupos extra,
  `-e FECHA` expira (YYYY-MM-DD), `-f DÍAS` expira contraseña, `-k SKEL`, `-r` sistema.
```bash
sudo useradd -m -s /bin/bash -c "Usuario App" -G sudo,www-data appuser
sudo passwd appuser
```

## `adduser` (alto nivel, interactivo en Debian)
Crea usuario y HOME con preguntas guiadas.
```bash
sudo adduser juan
```

## `usermod`
Modificar atributos.
- `-aG` añadir a grupos, `-s` shell, `-d` home, `-l` login, `-u` UID, `-g` GID, `-e` expira.
```bash
sudo usermod -aG docker,sudo juan
```

## `deluser` / `userdel`
Eliminar usuarios.
- `--remove-home` / `-r` borrar HOME, `--group` borrar grupo.
```bash
sudo deluser --remove-home juan
```

## `groupadd`, `groupdel`, `gpasswd`
Gestionar grupos y membresías.
```bash
sudo groupadd developers
sudo gpasswd -a juan developers
sudo gpasswd -d juan developers
```

## `passwd` y `chage`
- `passwd` cambiar contraseña; `passwd -l/-u` bloquear/desbloquear.
- `chage -l usuario` listar políticas, `-M` días máx, `-m` mín, `-W` aviso, `-E` expira.
```bash
sudo chage -M 90 -W 14 -E 2026-12-31 juan
```

## Archivos clave
- `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/sudoers` (editar con `visudo`).
