# Receta aportada — auditoría sin cambios en Termux (M5)

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux 0.118.3 · 2026-11-18 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Auditar el equipo y proponer mejoras sin cambiar nada, respetando los límites de Termux y sin borrar nada sin evidencia de uso.

## Pasos

1. En Termux no hay `systemd`, `systemctl` ni `service`; no borrar nada sin evidencia de uso.
2. Paquetes actualizables: `termux-info` muestra la lista (`pkg list-upgradable` no existe en Termux 0.118.3). Simular siempre con `apt-get -s upgrade` antes de proponer cambios.
3. Identidad del SO: `getprop ro.build.version.release`, `ro.build.version.security_patch`, `ro.build.display.id` y `ro.product.model`.
4. Procesos: `ps -A -o pid,user,comm`; no usar patrones que coincidan con el propio comando.
5. Límites detectados: `/proc/uptime` da `Permission denied`; `/sys/class/thermal/thermal_zone*/temp` no es legible; no hay `DISPLAY`/`WAYLAND_DISPLAY`, `xdg-user-dir`, `gio` ni `desktop-file-validate`. Marcar como no verificado, nunca rellenar.
6. Resultado: informe priorizado con beneficio, riesgo, reversibilidad y coste, con cero cambios aplicados. Las cachés pequeñas no justifican limpieza en un disco con poco espacio libre.
7. Proyecto persistente en `{{PROYECTOS}}/<proyecto>/` con `ESTADO.md`, `DECISIONES.md`, `DESCRIPCION.md` e `INFORME.md`.

## Cómo se comprueba

El informe existe en la carpeta persistente, cada dato tiene su orden de origen, no se aplicó ningún cambio y los límites figuran como no verificados.

## Lo que no se ha verificado

Soporte del fabricante, batería, temperatura y todo lo que Termux no puede leer; la auditoría no aplicó ninguna mejora.
