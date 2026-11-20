# Receta aportada — enrutado determinista de agentes

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · OpenCode v2.0.31 · 2026-11-19 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Los modelos pequeños eligen mal el subagente o se inventan uno. Se quiere una decisión determinista antes de delegar, y crear el agente que falte sin sobrescribir nada.

## Pasos

1. Script en `<CONFIG>/herramientas/orquestar.py`; tabla en `<CONFIG>/orquestador.json`. Python 3 estándar, sin dependencias externas.
2. Antes de delegar: `python <CONFIG>/herramientas/orquestar.py --pedido "<petición>"`. Devuelve JSON con `accion`: `usar`, `crear`, `conflicto` o `sin_ruta`.
3. Códigos de salida: `0` para `usar`/`crear`/`listar`; `3` sin ruta; `4` conflicto; `5` fallo en `--comprobar`; `6` capacidad no encontrada; `7` ID inválido o duplicado.
4. El orden de `routes` manda. Normalizar a minúsculas sin acentos y comparar con `re.search`.
5. Si el agente no existe, el script escribe el `.md` desde la ruta y ejecuta `opencode reload`; nunca sobrescribe un archivo existente. `--sin-recargar` es solo para pruebas.
6. Necesidad nueva: `--nueva-ruta --id <kebab-case> --keywords 'a|b' --capacidad <etiqueta> --descripcion '...' --cuerpo '...'`. La capacidad elige el primer modelo del catálogo que la contiene.
7. Pruebas aisladas con `--config` y `--agents-dir` temporales: dejan intacta la configuración real; `--comprobar` sobre la real devolvió cinco `ok`. La rama con recarga se probó con configuración temporal (`recarga: true`, salida `0`).
8. El script crea el archivo de forma determinista; el criterio de diseño sigue siendo del agente arquitecto cuando la petición es ambigua.

## Cómo se comprueba

`--pedido` con una petición conocida devuelve el agente esperado y el código `0`; `--comprobar` devuelve `ok` para cada ruta; una ruta nueva se prueba primero con `--config` temporal y después con `--pedido`.

## Lo que no se ha verificado

Reejecución en la sesión actual: el intérprete de Python no está en `PATH` hoy, aunque el registro de la instalación lo daba por presente. La idea es generalizable, pero exige comprobar el runtime antes de delegar.
