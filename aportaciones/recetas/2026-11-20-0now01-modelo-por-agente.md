# Receta aportada — modelo por agente en OpenCode v2

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · OpenCode v2.0.31 · 2026-11-19 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Repartir modelos por agente (económico, normal, potente, visión) sin fijar un modelo global que quite al usuario el control de su sesión, y evitar fallos al pasar imágenes.

## Pasos

1. Campo `model` en el frontmatter del agente: `proveedor/modelo#variante`, según la documentación v2 de agentes. Ejemplo: `model: opencode/tidepool-alpha-free#high` en `<CONFIG>/agents/<agente>.md`.
2. Tras editar agentes, ejecutar `opencode reload`; sin eso `opencode debug agents` sigue mostrando `request.body` vacío. Con recarga, muestra `model: {id, providerID, variant}` por agente.
3. Un subagentes usa su modelo configurado cuando se lanza con la herramienta `subagent` desde el hilo principal. `opencode run --agent <id>` no cambia el modelo de la sesión: con imágenes, el adjunto se valida contra el modelo de la sesión y falla si es de texto.
4. Para imágenes, el hilo principal pasa la ruta del archivo al subagente de visión; este la abre con `read` (PNG/JPEG/GIF/WebP, hasta 20 MiB) y describe con su modelo VL. Adjuntar el archivo a una sesión de modelo de texto da «this model does not support image input».
5. No fijar `model` global en `<CONFIG>/opencode.json`: se conserva el modelo que la persona ya usa en la sesión.
6. Disponibilidad comprobada en ese entorno: un modelo devolvió `403 This model is not available in your country`; otros cinco (flash, razonamiento y visión) respondieron. Comprobar la disponibilidad real antes de asignarlos.

## Cómo se comprueba

`opencode debug agents` muestra el modelo resuelto de cada agente tras la recarga; una delegación con imagen por ruta de archivo devuelve una descripción, y la misma imagen adjunta a una sesión de texto falla con el mensaje esperado.

## Lo que no se ha verificado

Disponibilidad de los modelos en otros países o cuentas; la lista de modelos de ese entorno puede cambiar.
