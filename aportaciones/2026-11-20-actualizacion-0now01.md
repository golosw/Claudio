# Retorno — actualización · kit v3.4 · 2026-11-20

## Sistema

| Dato | Valor |
|---|---|
| SO y versión | Android 13 (SDK 33), según `getprop ro.build.version.release` y `ro.build.version.sdk`; kernel `4.19.191-g6e5440f6a542` (`uname -r`). El registro previo de la instalación declaraba otro equipo y Android 16/SDK 36: dato en conflicto, sin resolver |
| Escritorio | Ninguno; sesión de terminal (Termux 0.118.3, `TERMUX_VERSION`) |
| Arquitectura · CPU · núcleos | `aarch64` (`uname -m`) · ARMv8 `CPU part 0xd05` (`/proc/cpuinfo`) · 8 núcleos (`nproc`) |
| RAM | Total ~5,6 GiB, disponibles ~0,83 GiB al medir (`/proc/meminfo`) |
| GPU · VRAM | No accesible desde la sesión; sin `/dev/kvm` ni `/proc/driver/nvidia`: sin verificar |
| Disco libre (aprox.) | ~4,1 GB libres de 107 GB, 97 % ocupado (`df -h ~`) |
| Virtualización | Sin KVM (`/dev/kvm` ausente); ninguna VM en uso |
| Runtimes | `curl`, `tar`, `diff`, `sha256sum`, `lsof`, `pkg`/`dpkg` y OpenCode v2.0.31; sin `git`, `python`/`python3` ni `node` en `PATH` al comprobarlo hoy (el registro previo y una receta daban Python 3.14.10 por instalado) |

## Asistente

| Dato | Valor |
|---|---|
| Programa y versión | OpenCode v2.0.31 (`opencode --version`) |
| Adaptador | Del kit: `adaptadores/opencode.md` (marcado «sin probar» en el kit; rutas y sintaxis verificadas aquí contra documentación v2) |
| Modelos usados en la instalación | Sesión principal: `opencode/big-pickle`; verificación no interactiva: modelo por defecto de la CLI (no aparece en la salida JSON); subagentes con modelo en el frontmatter (`hy4-flash-free`, `deepseek-v4.1-flash-free`, `tidepool-alpha-free#high`, `tidepool-alpha-free#max`, `nemotron-3.5-super-vl-free`) |
| Modo de permisos | Sin reglas `allow` ni `deny` en el archivo de configuración actual (`<CONFIG>/opencode.json` solo contiene compactado); permisos por defecto del programa, con preguntas como diálogos reales. El progreso previo afirmaba dos reglas `deny` que ya no están: dato en conflicto |

## Resultado

| Fase o módulo | Estado | Motivo si no quedó instalado |
|---|---|---|
| Fase 0 Detectar | instalado (dato de SO en conflicto al reverificar) | |
| Fase 1 Entrevista | instalado | |
| Fase 2 Copia de seguridad | instalado | Copia nueva verificada antes de actualizar: 62 ficheros |
| Fase 3 Instrucciones | fusionado | Cuatro adiciones de la plantilla v3.4; no se borró ninguna línea existente |
| Fase 4 Memoria, agentes y skills | instalado | 5 agentes y 6 skills cargados |
| Fase 5 Ajustes | instalado (config en conflicto con el progreso) | Archivo real `opencode.json`, `buffer: 25 %`; el progreso decía `.jsonc` y `20 %` |
| Fase 6 Módulos | instalado | M3, M5 y M8 instalados; resto omitido por perfil de terminal |
| Fase 7 Verificación | instalado | `opencode run --standalone --format json` devolvió la marca v3.4 |
| Fase 8 Informe | instalado | Registro persistente en `<CONFIG>/instalacion-claudio.md` |
| Fase 9 Retorno | fusionado | v3.4 añade recetas para compartir; este es el primer informe con ese apartado |
| M3 Personalidades | instalado | |
| M5 Auditoría | instalado | Solo inventario de lectura en este perfil |
| M8 Agentes avanzados | instalado | Cinco agentes con modelo propio y orquestador configurable |

Capacidades degradadas y plan B usado: línea de estado por plugin no verificable → aviso de cerrar el hilo pasados ~100 turnos; hooks gráficos/askpass → sin escritorio, M1 no elegido; umbral absoluto de compactado → `keep.tokens` y `buffer` porcentual; adaptador del kit sin probar → rutas y sintaxis contrastadas con la documentación v2; M1, M2, M4, M6 y M7 → omitidos (sin escritorio, navegador propio, systemd/KVM, runtime JS ni modelo local).

## Problemas que el kit no tenía previstos

| Archivo y paso del kit | Qué decía | Qué pasó | Cambio propuesto |
|---|---|---|---|
| `plantillas/instrucciones.md`, regla «Buscar antes que leer» del perfil `terminal` (Fase 3) | Usar `grep`/`glob` dirigidos | Las herramientas integradas basadas en ripgrep fallan con `unsupported platform for ripgrep: arm64-android`; funciona `grep` de la shell | Añadir al bloque `terminal`: si la búsqueda integrada falla en Android/arm64, usar `grep`/`find` de la shell |
| `actualizar.md`, paso 1 (skill `actualizar`) | «En este repositorio esa llamada devuelve 404 porque no hay objetos Release: cae a `/tags`» | `releases/latest` devolvió 200 con `tag_name: v3.4` el 2026-11-20 | Redactarlo como «puede devolver 404; si responde, usar `tag_name` y no consultar `/tags`» |
| `fases/05-ajustes.md`, validación de `<AJUSTES>` | Validar el archivo de ajustes y anotar los valores en el progreso | El archivo real es `opencode.json` (no `.jsonc`), con `buffer: 25 %`; el progreso decía otro nombre, `20 %` y dos reglas `deny` hoy ausentes | Al actualizar, releer `<AJUSTES>` y compararlo con el progreso; si diverge, anotar la divergencia en vez de dar por bueno el registro |
| `fases/00-detectar.md`, detección y registro | Anotar sistema y máquina al instalar | La detección actual dio otro modelo de equipo, Android 13/SDK 33 y kernel 4.19.191, frente al registro previo de otro equipo y Android 16/SDK 36; no se supo si cambió la máquina o el registro era erróneo | En cada actualización, reverificar SO/equipo y marcar «dato en conflicto» sin sobrescribir el registro hasta que la persona lo confirme |
| `modulos/08-agentes-avanzados.md`, orquestador opcional | La delegación se decide con el agente y sus modelos | La receta local de enrutado determinista requiere Python, hoy no presente en `PATH`; no se pudo reejecutar en esta actualización | Si se incorpora esa idea al kit, exigir y comprobar el runtime antes de delegar, con plan B de delegación manual |

## Solo al actualizar

Versión de partida → llegada: **v3.2 → v3.4**.

| Apartado | Aplicado | Por qué no, si no |
|---|---|---|
| `promptinicial.md` | Sí (marca) | |
| `adaptadores/opencode.md` | Sí (procedimiento) | |
| `fases/00-detectar.md` | Sí (procedimiento) | |
| `fases/03-instrucciones.md` | Sí (procedimiento) | |
| `plantillas/instrucciones.md` | Sí (4 fusiones) | |
| `fases/05-ajustes.md` | Sí (procedimiento) | |
| `fases/06-modulos.md` | Sí (procedimiento) | |
| `fases/09-retorno.md`, `plantillas/retorno.md` | Sí (este informe) | |
| `plantillas/receta-aportada.md` | Sí (11 recetas reescritas) | |
| M3, M5, M8 | Sí (procedimiento; instalación intacta) | |
| M1, M2, M4, M6, M7 | No | Módulos no instalados; M4 solo se ofrece en perfiles con escritorio |
| M4: `ui-esqueleto.html`, `ui-estilo.css` | No | Pertenecen a M4, no instalado |
| `actualizar.md` | No | Sin cambio de versión (3.2) |
| Adaptadores Claude Code, Codex CLI, Gemini CLI | No | No son el asistente de esta máquina |

Todo lo aplicado se conservó íntegro: no se borró ninguna memoria, agente, skill ni ajuste.

### Lo que este sistema tiene y podría interesar al kit

- **Enrutado determinista de agentes**: un script con tabla de rutas, códigos de salida y pruebas aisladas decide cuándo delegar y crea el agente que falte sin sobrescribirlo. Generaliza como ayuda opcional de M8 para modelos pequeños que fallan al elegir subagente; requiere un runtime comprobado antes de usarse.
- **Skill de memoria con reglas de uso**: además del vault, obliga a mirar diagnósticos previos antes de repetirlos y recetas antes de montar algo. Generaliza como refuerzo de la sección de memoria de la plantilla: convierte una recomendación en un paso verificable.
- **Comprobación de puertos locales**: verificar con `lsof` que el puerto está libre y que el servicio escucha solo en loopback antes de arrancar una vista previa. Generaliza para el perfil `terminal`, donde los visores locales son la única interfaz.
- **Ejecución no interactiva verificada**: `opencode run --standalone --format json` como prueba real de instrucciones, agentes y skills, sin abrir la TUI. Ya está en el kit, pero aquí se usó además como comprobación de la marca de versión tras la actualización.

## Recetas adjuntas

- `2026-11-20-agente-subagente.md`: dar de alta un subagente en OpenCode v2 con permisos y prueba real.
- `2026-11-20-agentes-skills.md`: agentes y skills nuevos en OpenCode v2 (módulo M8).
- `2026-11-20-orquestador-determinista.md`: enrutado y creación de agentes con un script y una tabla de rutas.
- `2026-11-20-modelo-por-agente.md`: modelo por agente, recarga y manejo de imágenes por ruta.
- `2026-11-20-personalidades.md`: personalidades con `AGENTS.md` local.
- `2026-11-20-instalacion-opencode-termux.md`: instalar el kit en OpenCode v2 / Termux sin `git`.
- `2026-11-20-apk-aarch64.md`: compilar una APK en Termux `aarch64` sin Gradle.
- `2026-11-20-recuperar-fuente-apk.md`: recuperar y recompilar el fuente de una APK con jadx y apktool.
- `2026-11-20-mcp-stl-local.md`: servidor MCP local y visor de STL sin dependencias.
- `2026-11-20-auditoria-android-termux.md`: inventario de sistema en Android/Termux.
- `2026-11-20-auditoria-m5-termux.md`: auditoría sin cambios del sistema en Termux (M5).

## Anonimización

Sustituciones hechas: 32 (sin detallar). Comprobación de texto: `grep -rniE` sobre el modelo de equipo, nombres de usuario y red, rutas personales, IPs, correos y nombres de proyectos concretos → sin coincidencias.
