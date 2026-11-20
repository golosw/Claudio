# Receta aportada — inventario de sistema en Android/Termux

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · 2026-11-18 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Saber en qué estado está el equipo antes de instalar o actualizar, con órdenes baratas y sin dar por buenos datos que no se pueden comprobar desde Termux.

## Pasos

1. `uname -a` puede decir `android14` mientras `getprop ro.build.version.release` dice otra cifra: anotar ambas, no diagnosticar.
2. Datos útiles: `/proc/meminfo`, `nproc`, `df -h ~`, `getprop ro.build.version.*`, `TERMUX_VERSION`, `pkg list-installed | wc -l` y `lsof -nP -iTCP -sTCP:LISTEN`.
3. No hay `ss`, `systemd`, `journalctl`, `nmcli` ni `xdg-user-dir`: no copiar tal cual las recetas de Linux de escritorio.
4. `ps -eo comm | sort | uniq -c | sort -rn` da una foto rápida de procesos propios sin confundirlos con los del sistema Android.
5. El soporte del fabricante y el estado de la batería no se pueden verificar desde Termux: marcar como sin verificar.
6. Guardar el resultado en un proyecto persistente con `ESTADO.md`, `DECISIONES.md`, `DESCRIPCION.md` e `INVENTARIO.md`, dentro de `{{PROYECTOS}}`.

## Cómo se comprueba

Cada dato del inventario va con la orden que lo produjo y su salida; las comprobaciones inaccesibles figuran como «sin verificar», y el inventario existe en la carpeta persistente.

## Lo que no se ha verificado

Soporte del fabricante, batería y cualquier dato del sistema al que Termux no tiene acceso.
