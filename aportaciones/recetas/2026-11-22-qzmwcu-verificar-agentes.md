# Receta aportada — Verificar agentes y skills en opencode v2

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Armbian OS 26.11.0 (Debian 12), aarch64 · opencode v2.3.2 · 2026-11-22 |
| Perfil de plataforma | terminal |

## Problema u objetivo

Comprobar de verdad que un agente o skill creado funciona, no solo que el archivo existe.

## Pasos

1. `opencode debug agents` lista agentes con `mode`, `model` y `permissions` resueltos; confirmar que aparece `subagent: deny`.
2. Un agente o skill creado en una sesión puede no verse en esa misma sesión: usar siempre una sesión nueva con `opencode run --format json "<encargo>"`.
3. Para probar un skill sin nombrarlo, preguntar algo que su `description` active y mirar en el JSON el evento `tool_use` con `tool: skill`.
4. Para probar un subagente, pedir al hilo principal «Usa el subagente <nombre>»; el JSON muestra `tool: subagent` y el resumen continúa en un evento posterior.
5. Formato de permisos de agente en v2: lista de `{ action, resource, effect }` en el frontmatter `permissions:` (no el objeto `permission` de v1).
6. Los artefactos de prueba (skill y proyecto efímero) se borran al terminar; los proyectos de prueba viven dentro de `{{PROYECTOS}}/`.

## Cómo se comprueba

`opencode debug agents` muestra los 4 agentes con `subagent: deny`; `opencode run --format json` con un encargo que active la `description` devuelve `tool_use: skill`, y con «Usa el subagente …» devuelve `tool_use: subagent` con el nombre correcto. La sesión nueva responde con la identidad del skill sin que se le nombre.

## Lo que no se ha verificado

El comportamiento en sesión interactiva (TUI) y en otras versiones de opencode; el registro puede variar si el agente se crea mientras la sesión está abierta.
