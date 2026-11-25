# Retorno — actualizacion · kit v3.4 · 2026-11-25

## Sistema

| Dato | Valor |
|---|---|
| SO y versión | Ubuntu 26.04.1 LTS (kernel 7.0) |
| Escritorio | GNOME (Wayland) |
| Arquitectura · CPU · núcleos | x86_64 · AMD Ryzen 5 5500U · 12 hilos |
| RAM | 14 GB |
| GPU · VRAM | integrada (sin GPU dedicada) |
| Disco libre (aprox.) | 120 GB |
| Virtualización | no consta (no se usó en esta actualización) |
| Runtimes | Python 3.14; sin Node |

## Asistente

| Dato | Valor |
|---|---|
| Programa y versión | Claude Code 2.1.326 |
| Adaptador | del kit |
| Modelos usados en la actualización | Sonnet (hilo principal y subagentes); Opus no hizo falta |
| Modo de permisos | ejecución desde la interfaz gráfica propia (permisos como diálogos) |

## Resultado

| Fase o módulo | Estado | Motivo si no quedó instalado |
|---|---|---|
| Instrucciones globales (marca v3.4 + regla «no escribir reglas allow propias») | fusionado | |
| M1 red de seguridad: veto de lectura Read/Grep/Glob/NotebookRead en el hook | instalado | Prueba real: 14/14 casos propios, 310/310 de la batería previa, y un `claude -p` real bloqueado |
| M4 interfaz gráfica adaptada a v3.4 (4 etapas) | instalado | Dos funciones no completas, ver abajo |
| M2, M3, M5, M8 | sin cambio local | cambios del kit solo para Windows o equipos gestionados |
| M6, M7 | omitido | no estaban instalados |
| Despliegue de M4 sobre el servicio en uso | instalado | Con copia previa verificada; ver «Problemas» (1.er intento revertido solo) |

Funciones de M4 no completas: «Remote Control» como botón de la conversación (**no hecha**: el CLI en modo `-p` responde «isn't available in this environment») y «esfuerzo de razonamiento por conversación» (**parcial**: solo es global, en Ajustes).
Aspecto, voz y táctil: verificados con capturas automáticas y con un uso real desde el móvil; la voz queda sin verificar por el asistente.

Capacidades degradadas y plan B: Remote Control sigue disponible por la vía anterior (terminal con `--remote-control`, desde el menú ⋮).

## Problemas que el kit no tenía previstos

| Archivo y paso del kit | Qué decía | Qué pasó | Cambio propuesto |
|---|---|---|---|
| `modulos/04-interfaz.md`, construcción delegada en subagentes | Delegar la construcción en etapas | Con una interfaz que lanza un `claude -p` por mensaje, un subagente en segundo plano muere al cerrarse el turno (dos veces) | Decir que, dentro de una interfaz sobre el CLI, los subagentes de construcción van en primer plano y por etapas |
| `modulos/04-interfaz.md`, token por arranque en `/api` | Toda petición a `/api` y `/ws` exige token | La comprobación de salud de un script de despliegue (`curl /api/sistema`) recibió 403 y disparó una marcha atrás innecesaria | Añadir a la receta de despliegue: la comprobación envía `X-Token` leído de `datos/.token` |
| `modulos/04-interfaz.md`, pruebas de la UI | Probar en una instancia paralela con otro puerto | Importar `app` desde una prueba reescribe `datos/.token`; y `CLAUDIO_PUERTO` heredado del shell apuntaba al servicio de producción, así que una prueba golpeó el servicio real | Avisar de ambas cosas: no copiar nunca un `.token` entre carpetas y fijar `CLAUDIO_PUERTO` en cada orden de prueba |
| `plantillas/ui-esqueleto.html` y `ui-estilo.css` | Plantilla base de la interfaz | A 360 px la barra lateral queda oculta sin forma de abrirla, el botón de enviar queda cortado y el panel derecho es transparente | Incluir en la plantilla un botón de cajón para la barra lateral y revisar esas reglas móviles |
| Escritura de JSON de datos | Escritura atómica | La implementación de partida usaba un temporal de nombre fijo; con escritores concurrentes, otro hilo leía el archivo vacío (`ajustes.json`) | Especificar temporal único (`mkstemp`) + `fsync` + `os.replace`, y una prueba con varios escritores y lectores |
| `actualizar.md`, paso de despliegue | — | Un reinicio del servicio corta la conversación que lo lanza; hubo que dejarlo como proceso desacoplado con marca de fin | Documentar el patrón: copia verificada + `systemd-run --user --on-active=` + archivo de fin + marcha atrás automática |

## Solo al actualizar

Versión de partida → llegada: v3.2 → v3.4

| Apartado | Aplicado | Por qué no, si no |
|---|---|---|
| Instrucciones globales | sí (fusionado, con copia previa y diff enseñado) | |
| Hook de seguridad con veto de lectura | sí | |
| M4 | sí | Remote Control no hecha; esfuerzo por conversación parcial |
| Fases 00/05/06, adaptador 3.3, M1-M3, M5, M8 | sin cambios | solo afectan a Windows o equipos gestionados |

### Lo que este sistema tiene y podría interesar al kit

- **Script de despliegue con marcha atrás**: copia de seguridad verificada con `diff`, copia del código sin tocar datos ni entorno, reinicio, comprobación real con token y restauración automática si no responde. Se generalizaría como plantilla de `actualizar` para cualquier servicio de usuario.
- **Batería de pruebas del veto de lectura**: 14 casos (bloquea y permite) lanzados contra el hook con su JSON de entrada; sirve para regresiones al cambiar el hook.

## Recetas adjuntas

Ninguna.
