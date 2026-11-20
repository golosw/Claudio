# Receta aportada — agentes y skills nuevos en OpenCode v2 (M8)

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · OpenCode v2.0.31 · 2026-11-19 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Crear agentes y skills del módulo M8 y probarlos de verdad, evitando que una sesión cacheada o un permiso denegado en silencio invalide la prueba.

## Pasos

1. La herramienta `subagent` del hilo solo acepta agentes ya listados por el sistema; con uno nuevo responde `Unknown agent`. Para probar `<CONFIG>/agents/<nombre>.md`, usar la CLI: `opencode run --standalone --auto --agent <nombre> "<encargo>"`.
2. `--standalone` evita tocar la sesión en marcha; `--auto` aprueba permisos no denegados explícitamente. Sin `--auto`, un permiso denegado se acepta en silencio y la prueba no sirve.
3. El encabezado `> <agente> · <modelo>` de la CLI muestra el modelo de la sesión, no siempre el del agente; comprobar la definición resuelta con `opencode debug agents`.
4. Frontmatter v2: `permissions` es una lista de `{action, resource, effect}`. El mapa `permission:` de v1 está obsoleto; no copiarlo a agentes nuevos. Referencia: `https://opencode.ai/docs/agents/`.
5. Un skill nuevo solo se descubre en sesiones nuevas: probarlo con `opencode run` y un encargo que no nombre el skill.
6. En `arm64-android`, las herramientas `glob`/`grep` basadas en ripgrep fallan con `unsupported platform for ripgrep`: usar `ls`, `find` y `grep` de la shell.
7. El servicio en segundo plano cachea los agentes: justo después de crear los `.md`, `opencode debug agents` no los lista; `opencode service restart` y volver a listar los muestra. `--standalone` no es un flag válido de `debug`.

## Cómo se comprueba

Tras reiniciar el servicio, `opencode debug agents` lista los agentes con su modelo y `subagent: deny`; una ejecución `opencode run --standalone --auto --agent <nombre>` devuelve la respuesta del encargo, y un skill nuevo se usa en una sesión nueva sin nombrarlo.

## Lo que no se ha verificado

Otros programas distintos de OpenCode v2 y la recarga sin reiniciar el servicio.
