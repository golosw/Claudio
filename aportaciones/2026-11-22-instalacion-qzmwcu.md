# Retorno — instalación · kit 3.4 · 2026-11-22

## Sistema

| Dato | Valor |
|---|---|
| SO y versión | Armbian OS 26.11.0 (Debian 12 bookworm), kernel 6.12.117-ophub |
| Escritorio | ninguno (sin `DISPLAY`/`WAYLAND_DISPLAY`); máquina accedida en remoto |
| Arquitectura · CPU · núcleos | aarch64 · 4× Cortex-A53 @1.2 GHz |
| RAM | 1.8 GiB (1.2 GiB disponible), sin swap |
| GPU · VRAM | sin GPU detectada (`lima`: sin regulator mali); VRAM no verificable |
| Disco libre (aprox.) | 11 GB libres de 14 GB |
| Virtualización | `/dev/kvm` accesible para <usuario>; sin contenedores |
| Runtimes | Python 3.11.2, git 2.39.5; sin Node/npm/docker/podman |

## Asistente

| Dato | Valor |
|---|---|
| Programa y versión | opencode v2.3.2 (ELF nativo en `/usr/local/bin/opencode`) |
| Adaptador | del kit (`adaptadores/opencode.md` v3.3) |
| Modelos usados en la instalación | `opencode/big-pickle` (hilo principal y verificador), `opencode/nemotron-3.5-lightning-free` (explorador), `opencode/flint-40b-free` (arquitecto-agentes) |
| Modo de permisos | global por defecto; `deny` de `subagent` en los 4 agentes; sin hook M1 |

## Resultado

| Fase o módulo | Estado | Motivo si no quedó instalado |
|---|---|---|
| Fase 0 · Detectar | instalado | |
| Fase 1 · Entrevista | instalado | |
| Fase 2 · Copia de seguridad | instalado | |
| Fase 3 · Instrucciones globales | instalado | |
| Fase 4 · Memoria, agentes y skills | instalado | |
| Fase 5 · Ajustes, línea de estado y modelo | omitido | El usuario rechazó el diff de compactado; sin TUI ni Node para un plugin de línea de estado |
| Fase 6 · Módulos, ejecutable estable, accesos | instalado | Accesos directos no aplican en perfil terminal |
| Fase 7 · Verificación | instalado | |
| Fase 8 · Informe | instalado | |
| M3 Personalidades auxiliares | instalado | <personalidad>, con fuentes de repositorios, issues y changelogs |
| M5 Auditoría del sistema | instalado | Solo inventario: 6 hallazgos, 0 cambios aplicados |
| M8 Agentes avanzados | instalado | 2 agentes + 2 skills, probados con encargos reales |
| M1 / M2 / M4 / M6 / M7 | omitido | Sin escritorio / sin Chromium headless ni Node / sin sesión gráfica y canal de permisos / no elegido / depende de M4 |

Capacidades degradadas y plan B usado: línea de estado sin verificar (aviso de cerrar hilo a ~100 turnos en las instrucciones globales); canal de permisos interactivo sin confirmar (reglas `deny` de agente verificadas, sin hook M1); `opencode run` sí resultó trazable con `--format json`; M4 y accesos directos no aplican en perfil terminal.

## Problemas que el kit no tenía previstos

| Archivo y paso del kit | Qué decía | Qué pasó | Cambio propuesto |
|---|---|---|---|
| `adaptadores/opencode.md:39,53` — término `<NO_INTERACTIVO>` y capacidad «No interactivo trazable» | «formato de eventos JSON sin confirmar» / «salida estructurada sin confirmar; si no la hay, verificación manual» | `opencode run --format json` emitió `step_start`, `text`, `tool_use` y `step_finish` con `cost` y `tokens`; la salida se guardó en archivo con marca de fin | Marcar ambos como confirmado en v2.3.2 y fijar `opencode run --format json` como `<NO_INTERACTIVO>` |
| `adaptadores/opencode.md:47` — capacidad «Subagentes» | Frontmatter con `description`, `mode: subagent`, `model`, `permissions` | En v2.3.2 `permissions` es una lista de `{ action, resource, effect }`; el objeto `permission` de v1 no aplica. `opencode debug agents` resolvió los 4 agentes y mostró `subagent: deny` | Añadir el formato concreto de la lista y `opencode debug agents` a las órdenes de verificación |
| `fases/06-modulos.md:49` — ejecutable estable | `env -i HOME=$HOME PATH=/usr/bin:/bin ~/.local/bin/<CLI> --version` «o la ruta fija que dé el adaptador» | El adaptador no da ruta fija: el ejecutable es un ELF nativo en `/usr/local/bin/opencode`, no hay `~/.local/bin`, y funciona con entorno vacío | En el adaptador, documentar la ruta fija nativa cuando no haya lanzador y comprobarla con `readlink -f` antes de decidir |

## Recetas adjuntas

- `2026-11-22-verificar-agentes.md`: cómo probar agentes y skills de opencode v2 en sesiones nuevas.
- `2026-11-22-inventario-terminal.md`: cómo inventariar un equipo en perfil terminal sin cambiar nada.

## Anonimización

Sustituciones hechas: 5 (sin detallar).
