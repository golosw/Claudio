# Receta aportada — Inventario en perfil terminal

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Armbian OS 26.11.0 (Debian 12), aarch64 · opencode v2.3.2 · 2026-11-22 |
| Perfil de plataforma | terminal |

## Problema u objetivo

Recoger el estado real de la máquina antes de trabajar en ella, sin escribir fuera del proyecto ni tocar servicios.

## Pasos

1. Crear el proyecto persistente `{{PROYECTOS}}/<proyecto>/` con `ESTADO.md`, `DECISIONES.md`, `DESCRIPCION.md` e `INFORME.md`.
2. Comandos base: `/etc/os-release`, `uname -a`, `lscpu`, `free -h`, `df -h /`, `systemd-analyze`, `systemd-analyze blame`, `systemctl list-units --type=service --state=running`, `systemctl --failed`, `journalctl -p err -b`, `ss -lntup`, `nmcli dev status`, `apt-get -s upgrade`.
3. En terminal no hay `xdg-user-dir` ni escritorio: no busques lanzadores ni interfaz; anota «no aplica».
4. `/sys/class/thermal/thermal_zone*/temp` da miligrados; sin umbral verificado para la placa, no afirmes sobrecalentamiento.
5. Si una wiki devuelve desafío, no afirmes fecha de fin de soporte: dilo como no verificado.
6. Cada hallazgo del informe lleva la orden que lo produce; nunca copies cifras de otro informe.

## Cómo se comprueba

El informe existe en la carpeta del proyecto y cada hallazgo tiene su evidencia. Las cifras se reproducen con las órdenes anteriores; el estado de lo que solo puede confirmar la persona queda marcado en `ESTADO.md`.

## Lo que no se ha verificado

El soporte del sistema (la fuente web devolvió un desafío), el consumo real en reposo y si el error de la tarjeta `mmc_select_hs200` afecta al rendimiento percibido.
