# Adaptador — opencode

Estado: **sin probar** con el kit. Tabla rellenada desde la documentación oficial
(`https://opencode.ai/docs/`: rules, config, agents, skills, permissions, plugins, commands). Lo marcado
«sin confirmar» no está en la documentación: compruébalo en la máquina (`opencode --help`, su carpeta)
antes de usarlo, y anota el resultado en el progreso.

Versión: lo que sigue corresponde a **opencode v2**. En v1 algunas claves se llamaban de otra forma
(compactado: `preserve_recent_tokens` → `keep.tokens`, `reserved` → `buffer`; guía de migración en
`https://opencode.ai/docs/migrate-v1`). Mira la versión instalada (`opencode --version`) antes de
escribir `<AJUSTES>`.

## Detección

Lo que sepas de ti mismo, o `opencode` en el árbol de procesos de tu shell. Variables propias: sin
confirmar.

## Atención: lee también la configuración de Claude Code

opencode, si no encuentra la suya, carga `~/.claude/CLAUDE.md` y las skills de `~/.claude/skills/`
(se desactiva con `OPENCODE_DISABLE_CLAUDE_CODE`, `…_PROMPT` o `…_SKILLS`). Si en la máquina ya hay una
instalación del kit para Claude Code, decide en la Fase 0 con el usuario:
- **compartir**: opencode usa la misma identidad y memoria (no crees `AGENTS.md` propio; las skills se
  cargan de `~/.claude/skills/`);
- **separar**: crea `~/.config/opencode/AGENTS.md` y pon las variables anteriores en su entorno, para
  que no se carguen las dos veces.

## Términos

| Término | Valor |
|---|---|
| `<CLI>` | `opencode` |
| `<CONFIG>` | `~/.config/opencode` |
| `<INSTRUCCIONES>` | `~/.config/opencode/AGENTS.md` |
| `<INSTR_LOCAL>` | `AGENTS.md` (si no existe, lee `CLAUDE.md`) |
| `<AJUSTES>` | `~/.config/opencode/opencode.json` (o `.jsonc`) |
| `<AGENTES>` | `~/.config/opencode/agents/<nombre>.md` |
| `<SKILLS>` | `~/.config/opencode/skills/<nombre>/SKILL.md` (también lee `~/.agents/skills/`) |
| `<NO_INTERACTIVO>` | `opencode run --format json "<encargo>"`: eventos `step_start`, `text`, `tool_use` y `step_finish` (con `cost` y `tokens`); confirmado en v2.3.2 (retorno 2026-11-22). Guarda la salida en un archivo |
| `<SESIONES>` | sin confirmar |

## Capacidades

| Capacidad | Estado | Cómo |
|---|---|---|
| Instrucciones globales | sí | `<INSTRUCCIONES>` |
| Subagentes | sí | `.md` con frontmatter `description`, `mode: subagent`, `model`, `permissions`. En v2.3.2 `permissions` es una **lista** de `{ action, resource, effect }` (p. ej. `action: subagent`, `resource: "*"`, `effect: deny`); el objeto `permission` de v1 no se aplica. Se comprueba con `opencode debug agents`, que lista cada agente con su modo, modelo y permisos resueltos. Un agente creado en la sesión en curso puede no verse: usa una sesión nueva |
| Skills | sí | Carpeta con `SKILL.md` (`name`, `description`) |
| Hooks que bloquean | parcial | Reglas de `permissions` en `<AJUSTES>` (`allow`/`ask`/`deny` por herramienta y patrón) o un plugin que deniega en la evaluación de permisos. Sirve para la red de seguridad de M1 si la lista se expresa como reglas `deny` |
| Línea de estado | sin verificar | La documentación de plugins (`https://opencode.ai/docs/plugins/`) admite plugins solo de la TUI; un usuario informa de un pie de estado por «slots» de plugin en v2, pero no está verificado en la documentación. Si no se puede comprobar en la máquina, plan B |
| Compactado | sí | Verificado en `https://opencode.ai/docs/compaction/`: `compaction.auto` (por defecto `true`), `compaction.background` (`true`), `compaction.keep.tokens` (tokens recientes que se conservan, 15000) y `compaction.buffer` (parte de la ventana que queda libre; por defecto `"15%"`, mínimo ~16k; cuanto mayor, antes compacta). No hay umbral absoluto: aplica el equivalente de la Fase 5 (`buffer` = ventana − 150 000) |
| Modelo por nivel | sí | `"model": "proveedor/modelo"` global y por agente. Los niveles los elige el usuario según su proveedor (también modelos locales: Ollama, LM Studio, vLLM) |
| No interactivo trazable | parcial | `opencode run` existe; salida estructurada sin confirmar. Si no la hay, verificación manual |
| Canal de permisos | sin confirmar | M4 no se ofrece mientras no se compruebe |

## Perfil `terminal` (Termux, SSH)

opencode funciona en Termux (informado con v2.0.31). Sin escritorio: no se ofrecen M4 ni M7, y los
paquetes se instalan con `pkg`. Lo que la plantilla marca para otros perfiles se quita en la Fase 3.

## Órdenes de verificación

Las de las fases, con `<NO_INTERACTIVO>` (en una sesión nueva, para ver agentes y skills recién creados) y `opencode debug agents` para los agentes; si no, verificación manual (el usuario
abre `opencode`, hace la pregunta y pega la respuesta).
