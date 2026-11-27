# MANIFIESTO — Claudio v3.5

Una fila por apartado. La columna **versión** sube cuando cambia el contenido de ese archivo: una
actualización (`actualizar.md`) compara esta tabla con la de la versión instalada y aplica solo
los apartados cuya versión ha subido o que son nuevos. De los de tipo **adaptador** solo cuenta el
del asistente instalado.

| Apartado | Archivo | Versión | Tipo |
|---|---|---|---|
| Entrada | `promptinicial.md` | 3.5 | obligatorio |
| Adaptadores: términos y capacidades | `adaptadores/README.md` | 3.0 | obligatorio |
| Adaptador Claude Code (recomendado, probado) | `adaptadores/claude-code.md` | 3.3 | adaptador |
| Adaptador Codex CLI (sin probar) | `adaptadores/codex.md` | 3.0 | adaptador |
| Adaptador Gemini CLI (sin probar) | `adaptadores/gemini-cli.md` | 3.0 | adaptador |
| Adaptador opencode (probado en v2.3.2, un equipo) | `adaptadores/opencode.md` | 3.5 | adaptador |
| Detectar | `fases/00-detectar.md` | 3.5 | obligatorio |
| Entrevista | `fases/01-entrevista.md` | 3.0 | obligatorio |
| Copia de seguridad | `fases/02-copia-seguridad.md` | 3.0 | obligatorio |
| Instrucciones globales | `fases/03-instrucciones.md` | 3.3 | obligatorio |
| Plantilla de instrucciones | `plantillas/instrucciones.md` | 3.5 | obligatorio |
| Memoria, agentes y skills | `fases/04-memoria-agentes-skills.md` | 3.0 | obligatorio |
| Ajustes, línea de estado y modelo | `fases/05-ajustes.md` | 3.3 | obligatorio |
| Módulos, ejecutable estable, accesos | `fases/06-modulos.md` | 3.5 | obligatorio |
| Verificación | `fases/07-verificacion.md` | 3.0 | obligatorio |
| Informe | `fases/08-informe.md` | 3.2 | obligatorio |
| Informe de retorno (se ofrece, opcional para el usuario) | `fases/09-retorno.md` | 3.3 | obligatorio |
| Plantilla del informe de retorno | `plantillas/retorno.md` | 3.3 | obligatorio |
| Plantilla de receta aportada | `plantillas/receta-aportada.md` | 3.3 | obligatorio |
| Actualizar desde el repositorio | `actualizar.md` | 3.5 | obligatorio |
| M1 Elevación gráfica y red de seguridad | `modulos/01-elevacion-seguridad.md` | 3.3 | opcional |
| M2 Navegador automatizable | `modulos/02-navegador.md` | 3.3 | opcional |
| M3 Personalidades auxiliares | `modulos/03-personalidades.md` | 3.3 | opcional |
| M4 Interfaz gráfica | `modulos/04-interfaz.md` | 3.4 | opcional |
| Plantilla de aspecto de la interfaz (HTML) | `plantillas/ui-esqueleto.html` | 3.4 | M4 |
| Plantilla de aspecto de la interfaz (CSS) | `plantillas/ui-estilo.css` | 3.4 | M4 |
| M5 Auditoría del sistema | `modulos/05-auditoria.md` | 3.3 | opcional |
| M6 Permisos ampliados | `modulos/06-permisos.md` | 3.3 | opcional |
| M7 Modelo local pequeño | `modulos/07-modelo-local.md` | 3.3 | opcional |
| M8 Agentes avanzados | `modulos/08-agentes-avanzados.md` | 3.3 | opcional |

Repositorio: `https://github.com/golosw/Claudio`. Cada versión publicada es una etiqueta `vX.Y`
(o `vX.Y.Z`); la rama principal es borrador y las instalaciones no la usan.
