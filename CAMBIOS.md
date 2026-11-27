# CAMBIOS

## v3.5

Origen: los informes de retorno de las propuestas #3 (actualización v3.4 en Android/Termux) y #5
(instalación v3.4 en Armbian), ambos con opencode v2. Lo aplicado es lo que consta en esos informes;
no se ha reproducido en una máquina propia.

- **Adaptador opencode** (`adaptadores/opencode.md`): `opencode run --format json` confirmado como modo no
  interactivo (v2.3.2); `permissions` de los agentes es una lista `{ action, resource, effect }` y se
  comprueba con `opencode debug agents`.
- **Ejecutable estable** (`fases/06-modulos.md`): si el CLI ya es nativo y está en una ruta fija, no hace
  falta lanzador.
- **Perfil `terminal`** (`plantillas/instrucciones.md`): si la búsqueda integrada falla (ripgrep en
  Android/arm64), usar `grep`/`find` de la shell.
- **Actualizar** (`actualizar.md`): `releases/latest` solo cae a `/tags` si da 404.
- **Identificador de equipo** (`promptinicial.md`, `fases/00-detectar.md`, `actualizar.md`): el registro de
  instalación lleva el equipo; si no coincide con la máquina, es de otro equipo y se pregunta antes de seguir.

## v3.4

Origen: la prueba de M4 desde cero (2026-11-19): salió una página con 61 líneas de CSS, sin selector
de modelo, panel de agentes ni ajustes, y el contexto de una instalación real (sesión larga) lo empeora.

- **Interfaz** (`modulos/04-interfaz.md`): el aspecto deja de ser un mínimo escrito y pasa a copiarse de
  `plantillas/ui-esqueleto.html` y `plantillas/ui-estilo.css` (los `id` son el contrato). La construcción
  en sesión o subagente dedicado pasa de consejo a obligatoria, con informe por función (hecha / parcial /
  no hecha) como puerta de salida. Criterios nuevos: aspecto, modelo, agentes y ajustes.

## v3.3

Origen: la incidencia #1 (opencode v2 en Termux) y el informe de retorno de la propuesta #2
(Windows con Claude Code), más las quejas sobre la interfaz de una instalación nueva.

- **Perfiles de plataforma** (`fases/00-detectar.md`, `fases/06-modulos.md`, `fases/03-instrucciones.md`,
  `plantillas/instrucciones.md`, línea «Para:» de cada módulo): la Fase 0 fija un perfil (`linux`,
  `macos`, `windows`, `terminal`) y detecta si el equipo está gestionado por una organización. La Fase 6
  solo ofrece los módulos de ese perfil y dice por qué omite los demás. La plantilla marca con
  `<!-- perfiles: … -->` las reglas que solo valen en algunos (servicios y autoarranque, origen de las
  herramientas, máquinas virtuales, elevación) y la Fase 3 deja solo los del perfil detectado.
- **Windows** (`fases/05-ajustes.md`, `adaptadores/claude-code.md`, `modulos/02-navegador.md`,
  `modulos/01-elevacion-seguridad.md`): scripts `.ps1` con BOM UTF-8 y salida UTF-8 en sus pruebas;
  `MSYS_NO_PATHCONV=1` para `claude -p /autocompact` en Git Bash; lanzador `navegador.cmd` y comillas
  simples en el JS de `evaluar` con PowerShell 5.1; elevación con UAC en un solo script.
- **Equipos gestionados** (`modulos/05-auditoria.md`): lo corporativo se inventaría pero no se toca;
  lo que requiere administrador va en un solo script con log y marca de fin.
- **Permisos propios** (`fases/05-ajustes.md`, `plantillas/instrucciones.md`, adaptador de Claude Code):
  el asistente no escribe reglas `allow` para sí mismo; las propone y el usuario las añade.
- **Agentes avanzados** (`modulos/08-agentes-avanzados.md`, adaptador de Claude Code): si el programa no
  deja a un subagente escribir en `<CONFIG>`, escribe en una carpeta de preparación y el hilo principal
  la copia; lo creado se prueba en una sesión nueva.
- **Interfaz** (`modulos/04-interfaz.md`): conversaciones en paralelo (cambiar de conversación no para la
  anterior; turnos por conversación, cola con tope según la RAM, permisos en segundo plano en cola);
  aspecto mínimo exigible; selector de modelo, lista de agentes y panel de ajustes; el veto de archivos
  vale también para las herramientas del CLI; el árbol parte de `{{PROYECTOS}}`; el botón del kit
  explica qué instala antes de lanzarlo.
- **opencode v2** (`adaptadores/opencode.md`): compactado con `compaction.keep.tokens` y
  `compaction.buffer` (verificado en la documentación oficial), equivalencias desde v1, perfil
  `terminal` (Termux). El pie de estado por plugin queda «sin verificar». La Fase 5 explica cómo
  traducir el umbral de 150 000 tokens a un margen. El adaptador sigue sin probar con el kit.
- **Retorno** (`fases/09-retorno.md`, `plantillas/retorno.md`, `aportaciones/README.md`): si se envía
  como incidencia, el cuerpo es el informe entero. Nuevo punto 3, **recetas para compartir**: el usuario
  elige qué recetas o diagnósticos de infraestructura adjuntar; se reescriben con la nueva
  `plantillas/receta-aportada.md`, se anonimizan y van en `aportaciones/recetas/`. En el repositorio
  son material de estudio: nada se incorpora sin reproducirlo y reescribirlo.
- Plantilla: marca `v3.3`.

## v3.2

- **Informe de retorno** (`fases/09-retorno.md`, `plantillas/retorno.md`, nuevos): al terminar una
  instalación o una actualización se ofrece crear un `.md` con el sistema (SO, máquina, asistente,
  modelos), el estado de cada fase y módulo, los problemas que el kit no tenía previstos con el cambio
  propuesto y, al actualizar, lo aplicado y lo no aplicado y lo que el sistema tiene que podría servir al
  kit. Se anonimiza, el usuario lo revisa entero y puede enviarlo como propuesta (pull request) a
  `aportaciones/`, o como incidencia si no usa `gh`.
- `actualizar.md`: paso 7, ofrece el informe de retorno.
- `promptinicial.md`: fase 9 en la tabla; `fases/08-informe.md` remite a ella. Plantilla: marca `v3.2`.

## v3.1

- **Interfaces heredadas de v2.x** (`modulos/04-interfaz.md`, `fases/00-detectar.md`): al pasar a la v3,
  el aviso del kit de una interfaz v2.x seguía mirando el servidor antiguo y el botón «update disponible»
  llevaba a él. Ahora la Fase 0 marca esos canales como «a adaptar» y M4 dice cómo: aviso del kit contra
  el repositorio y autoactualización por servidor desactivada (salvo que el usuario publique en él).
  Quien ya instaló la v3.0 sobre una v2.x: «busca actualizaciones del kit» y aplica M4.
- **Plantilla**: solo cambia la marca a `v3.1`.

## v3.0

**Cambio de formato.** El kit deja de ser un único prompt con código de referencia adjunto y pasa a ser un
conjunto de especificaciones en Markdown, un archivo por apartado, que empieza por `promptinicial.md`.
**No lleva código**: cada instalación escribe el suyo, adaptado a su máquina, y lo que debe coincidir es
el comportamiento que piden los criterios de aceptación.

- **Distribución por GitHub**: las actualizaciones se buscan en las etiquetas publicadas del repositorio
  (`actualizar.md`, instalado como skill `actualizar`), no en un servidor local.
- **`MANIFIESTO.md`** con una versión por apartado: una actualización solo revisa lo que ha cambiado.
- **Universal, con adaptadores**: las fases y módulos hablan de capacidades y términos (`<CONFIG>`,
  `<INSTRUCCIONES>`, `<AJUSTES>`…), y `adaptadores/` los traduce para cada asistente de terminal. Claude
  Code es el recomendado y el único probado; Codex CLI, Gemini CLI y opencode tienen adaptador sin probar,
  y cualquier otro usa el adaptador genérico. Cada capacidad que falta tiene un plan B visible.
- **Renombrados**: `fases/03-claude-md.md` → `fases/03-instrucciones.md`, `fases/05-settings.md` →
  `fases/05-ajustes.md`, `plantillas/CLAUDE.md` → `plantillas/instrucciones.md`.
- **Progreso persistente** en `<CONFIG>/instalacion-claudio.md`: una instalación cortada se reanuda sin
  repetir preguntas.
- **Criterios de aceptación** en cada fase y módulo; la Fase 7 los repasa contra el manifiesto.
- **Poda de la base**: los subagentes base son `explorador` y `verificador`. `implementador`,
  `arquitecto-agentes` y las skills `orquestar` y `nuevo-agente` pasan al módulo opcional **M8 Agentes
  avanzados**. En instalaciones previas que ya los tengan, se conservan: no se borra nada.
- **Skill `actualizar`** nueva en la base.
- **Interfaz (M4)**: el aviso «kit vX disponible» consulta GitHub.
- **Verificación**: las pruebas de sesión usan el modo no interactivo del asistente (`<NO_INTERACTIVO>`)
  directamente, sin script auxiliar; si no lo tiene, verificación manual.
- **Sustituciones** (para la fusión de la Fase 3): en `<INSTRUCCIONES>`, la tabla de delegación pasa a
  usar niveles (económico/normal/potente) sustituidos por los modelos del adaptador, y a
  listar solo los agentes base, con una línea para los de M8 si está instalado. La sección del kit
  distribuible, si existía, es propia de quien distribuye y se conserva tal cual.

### Desde v2.x

Las instalaciones v2.x traían código copiado de las implementaciones de referencia (línea de estado,
hooks, interfaz, navegador). Es trabajo instalado y se conserva. La v3.0 lo compara con las
especificaciones nuevas y propone solo lo que falte.

Contenido heredado de la serie 2 que sigue vigente (ahora como especificación):
- actualización sin destruir;
- elevación gráfica y red de seguridad del hook;
- navegador automatizable;
- personalidades;
- interfaz con permisos como diálogos y seguridad de loopback;
- auditoría del sistema;
- permisos ampliados;
- guardianes para modelo local;
- línea de estado con compactado temprano y `opusplan` (v2.8).
