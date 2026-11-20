# Receta aportada — agente subagente en OpenCode v2

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · OpenCode v2.0.31 · 2026-11-18 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Dar de alta un subagente nuevo en OpenCode v2 con permisos explícitos y demostrar que se carga y responde.

## Pasos

1. Crear `<CONFIG>/agents/nombre.md` (global) o `<PROYECTO>/.opencode/agents/nombre.md` (por proyecto).
2. Frontmatter mínimo: `description` (cuándo usarlo, en español), `mode: subagent` y `permissions` como lista:

```yaml
permissions:
  - action: subagent
    resource: "*"
    effect: deny
```

3. Añadir `edit: deny` en los agentes de solo lectura. En v2, `edit` cubre `edit`, `write` y `patch`.
4. El cuerpo dice explícitamente que no lanza otros agentes y qué devuelve (tabla de rutas, evidencia, etc.).
5. Probar con `opencode run --standalone --agent nombre --format json "encargo pequeño"` y guardar la salida como evidencia.
6. Comprobar la carga con `opencode debug agents`.

Alias de permisos de v1 (`task`, `bash`, `permission`) ya no valen: son `subagent`, `shell` y `permissions`.

## Cómo se comprueba

`opencode debug agents` lista el agente con sus permisos efectivos, y la ejecución de prueba devuelve una respuesta real del encargo.

## Lo que no se ha verificado

Carga en una sesión ya abierta sin `opencode reload`: los cambios de agentes pueden exigir recarga o sesión nueva.
