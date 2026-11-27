# Fase 6 — Módulos opcionales, ejecutable estable y accesos directos

## Elegir módulos

Pregúntame cuáles quiero con preguntas de selección múltiple (como mucho 4 opciones por pregunta:
agrúpalos en dos si hace falta). Marca como recomendados los que encajen con mi dominio y con la
máquina detectada. **Ofrece solo los módulos cuyo «Para:» incluye el perfil de plataforma de la Fase 0**
(columna «Perfiles»), y omite también los que necesitan una capacidad que el adaptador marca como no
disponible (cada módulo dice cuáles necesita). Los omitidos se nombran en una línea con el motivo, sin
preguntarlos.

| Módulo | Archivo | Perfiles | Cuándo recomendarlo |
|---|---|---|---|
| M1 Elevación gráfica y red de seguridad | `modulos/01-elevacion-seguridad.md` | linux (macos, windows: solo la red de seguridad) | Linux con escritorio |
| M2 Navegador automatizable | `modulos/02-navegador.md` | linux, macos, windows (terminal: solo con un Chromium headless instalable) | Investigación, desarrollo web |
| M3 Personalidades auxiliares | `modulos/03-personalidades.md` | todos | Investigación o roles distintos |
| M4 Interfaz gráfica | `modulos/04-interfaz.md` | linux, macos, windows | Con escritorio y usuario poco de terminal |
| M5 Auditoría del sistema | `modulos/05-auditoria.md` | todos (terminal: solo inventario) | **Equipo virgen**: recomendado y el primero |
| M6 Permisos ampliados | `modulos/06-permisos.md` | todos | Nunca recomendado, salvo equipo dedicado a reparar otros |
| M7 Modelo local pequeño | `modulos/07-modelo-local.md` | linux, macos, windows (necesita M4) | Solo si M4 hablará con un modelo local de 1-4B |
| M8 Agentes avanzados | `modulos/08-agentes-avanzados.md` | todos | Uso intensivo de desarrollo o de delegación |

En un equipo virgen, M5 va primero porque sus hallazgos (sistema sin soporte, poca RAM, sin
escritorio) cambian qué otros encajan. En un **equipo gestionado** (Fase 0), dilo al ofrecer M1, M5 y
M6: lo corporativo no se toca.

Para cada módulo elegido:
1. Lee **solo** su archivo.
2. Detecta si ya existe: si existe, verifícalo y complétalo, sin rehacerlo.
3. Constrúyelo según su especificación, con el código que encaje en esta máquina.
4. Pasa sus criterios de aceptación y anota la evidencia en `<CONFIG>/instalacion-claudio.md`.
5. Añade una línea en `## Módulos instalados` de `<INSTRUCCIONES>` (qué es y dónde vive su
   configuración) y una receta en `memoria/recetas/` con lo que costó resolver.

Los módulos no elegidos no se tocan, aunque ya estén instalados.

## Ejecutable estable (perfiles `linux` y `terminal` con systemd o servicios; siempre, antes de los accesos directos)

Por qué: los accesos directos, los servicios y la interfaz no cargan el `.bashrc`. Si `<CLI>` cambia
de sitio con una actualización (típico con npm/nvm), todo eso deja de funcionar.

- Mira cómo está instalado (`command -v <CLI>`, `readlink` del ejecutable) y sigue lo que diga el
  adaptador (puede recomendar su instalación nativa).
- Si no hay una ruta fija que se mantenga sola, crea en `~/.local/bin/<CLI>` un lanzador que busque la
  versión más reciente instalada (nativa o npm, con o sin nvm) y la ejecute sin depender del PATH
  interactivo, guardando antes lo que hubiera.
- Nunca `curl | sh`: un instalador se descarga a un archivo, se revisa y se ejecuta.

Aceptación: `env -i HOME=$HOME PATH=/usr/bin:/bin ~/.local/bin/<CLI> --version` funciona (o la ruta
fija que dé el adaptador). Si el ejecutable ya es nativo y está en una ruta fija del sistema (comprueba con
`readlink -f "$(command -v <CLI>)"`) y funciona con entorno vacío, no hace falta lanzador: anota esa ruta
y pasa la misma orden con ella.

## Accesos directos (siempre que haya sesión gráfica)

En el Escritorio (`xdg-user-dir DESKTOP`): un acceso "<nombre> CLI" que abre `<CLI>` en una
terminal con la carpeta de proyectos como directorio de trabajo y, si M4 está instalado, otro que
abre la interfaz.

- Ruta absoluta del ejecutable, sin fijar una versión concreta; directorio de trabajo explícito.
- Márcalos como de confianza si el escritorio lo exige.
- Antes, mira los accesos que ya haya: si alguno ya abre el asistente o la interfaz, no crees un
  duplicado. No sobrescribas uno distinto: enséñame el diff.
- En otro sistema, el equivalente (macOS: un `.command`; Windows: un acceso directo `.lnk` o un `.cmd`
  con la carpeta de trabajo fijada). Perfil `terminal`: no hay accesos directos; dilo en el informe.

Aceptación: `desktop-file-validate` sin errores; lanzado de verdad (`gio launch <archivo>`), aparece
un proceso `<CLI>` nuevo cuyo `/proc/<pid>/cwd` es la carpeta de proyectos (ciérralo por su PID).
Que se ven en el Escritorio lo confirma el usuario.
