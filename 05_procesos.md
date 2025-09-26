# Capítulo 5 — Procesos

## `ps`
- Estilo BSD: `ps aux` (todos, usuarios, formato amplio)
- Estilo Unix: `ps -ef`
- Formato: `ps -o pid,ppid,cmd,%cpu,%mem --sort=-%cpu`
```bash
ps aux --sort=-%mem | head
```

## `top` / `htop`
Monitor en tiempo real.
- `top -o %MEM` (ordenar), `htop` (interactivo).

## `pgrep` / `pkill`
Buscar/matar por nombre.
- `pgrep -a nginx`, `pkill -9 node`

## `kill`
Enviar señales (por defecto `TERM`).
- Señales comunes: `-TERM`, `-KILL`, `-HUP`, `-STOP`, `-CONT`.
```bash
kill -HUP $(pgrep nginx)
```

## `nice` / `renice`
Prioridad (niceness -20..19).
```bash
nice -n 10 comando
sudo renice -n -5 -p 1234
```

## `jobs`, `fg`, `bg`, `&`
Control de trabajos en la shell.
```bash
comando & ; jobs ; fg %1
```
