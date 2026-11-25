# Retorno — actualizacion · kit v3.4 · 2026-11-25

## Sistema

| Dato | Valor |
|---|---|
| SO y versión | Windows 11 Enterprise, build 26300 (equipo gestionado por una organización: WorkplaceJoined, MDM y SCCM) |
| Escritorio | Windows (interfaz ClaudioUI) |
| Arquitectura · CPU · núcleos | AMD64 · Intel Core i7-1255U · 10 núcleos / 12 hilos |
| RAM | 32 GB |
| GPU · VRAM | Intel Iris Xe (integrada) |
| Disco libre (aprox.) | ~630 GB |
| Virtualización | WSL disponible |
| Runtimes | PowerShell 5.1, git 2.56, Python 3.13.17; sin Node ni Docker |

## Asistente

| Dato | Valor |
|---|---|
| Programa y versión | Claude Code 2.1.328 dentro de ClaudioUI |
| Adaptador | del kit (`claude-code`) |
| Modelos usados en la actualización | Opus 5.5 (hilo principal); prueba de aceptación con `claude -p` |
| Modo de permisos | auto (clasificador) |

## Problemas que el kit no tenía previstos

| Archivo y paso del kit | Qué decía | Qué pasó | Cambio propuesto |
|---|---|---|---|
| Skill `actualizar`, paso 3 | Comparar `MANIFIESTO.md` y aplicar los apartados afectados | Al venir de v3.2, el registro de instalación no tenía perfil de plataforma ni indicación de equipo gestionado, y v3.4 hace depender de ellos varios bloques (elevación, M5). Hubo que detectarlos a mano (`dsregcmd /status`, inscripción MDM) | Al actualizar desde una versión sin perfiles, detectar el perfil y si el equipo está gestionado antes de comparar, y anotarlos en `instalacion-claudio.md` |
| Plantilla de instrucciones, bloque Windows | «En un equipo gestionado por la organización, no toques servicios…» | La línea de la máquina de la instalación v3.2 decía «uso exclusivo», sin mencionar la gestión corporativa, y quedaba contradictoria con la regla nueva | Que la plantilla tenga un hueco en «La máquina» para equipo gestionado (sí/no), rellenado en la Fase 0 |

## Solo al actualizar

Versión de partida → llegada: v3.2 → v3.4 (perfil `windows`). Copia de seguridad verificada antes de tocar nada (recuento de ficheros igual en copia y origen).

| Apartado | Aplicado | Por qué no, si no |
|---|---|---|
| Instrucciones (marca, elevación en un solo script UAC, no tocar lo corporativo, no escribirse reglas `allow`) | sí, fusionado | |
| `adaptadores/claude-code.md` / `fases/05-ajustes.md` | ya cumplía | `.ps1` de la línea de estado ya con BOM; margen de compactado ya configurado |
| M1 Elevación / red de seguridad | ya cumplía | `deny` y regla de confirmar borrados ya instaladas |
| M2 Navegador | ya cumplía | `.cmd` y comillas simples ya documentados |
| M3 Personalidades | sin cambios aplicables | |
| M4 Interfaz | no | La interfaz se actualiza con su propio instalador |
| M5 Auditoría | sí | Nota de equipo gestionado en el estado del proyecto |
| M6, M7 | no | Módulos no instalados |
| M8 Agentes avanzados | sí | Carpeta de preparación añadida a la definición del arquitecto de agentes |

Prueba de aceptación: sesión nueva `claude -p` preguntando por las dos reglas nuevas; citó la regla de no ampliarse permisos y la de un solo script elevado con log.

### Lo que este sistema tiene y podría interesar al kit

- **Nota de diagnóstico de la app de escritorio de Claude (MSIX) colgada tras suspensión**: dónde están sus logs en Windows, qué marcas mirar y cómo reiniciarla por PID sin tocar el CLI. Útil a cualquiera que tenga la app de escritorio y el CLI a la vez en Windows.

## Recetas adjuntas

- `2026-11-25-3gus6z-claude-desktop-colgado.md`: diagnóstico y reinicio limpio de la app de escritorio de Claude (MSIX) colgada tras suspensión, sin tocar el CLI.

## Anonimización

Sustituciones hechas: 0 (redactado ya sin datos personales; búsqueda de texto de comprobación sin coincidencias).
