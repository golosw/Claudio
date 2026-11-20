# Receta aportada — servidor MCP local y visor de STL sin dependencias

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · OpenCode v2.0.31 · 2026-11-19 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Generar y ver modelos STL en un equipo sin escritorio, sin FreeCAD de escritorio y sin dependencias de Python fuera de la biblioteca estándar.

## Pasos

1. FreeCAD de escritorio no tiene paquete en Termux y los AppImages no funcionan (sin FUSE, ARM). Instalar Python con `pkg install -y python` y escribir el servidor solo con biblioteca estándar.
2. Servidor MCP: JSON-RPC 2.0 delimitado por saltos de línea en stdin/stdout. Responder `initialize` con `protocolVersion` (eco), `capabilities.tools`, `serverInfo` e `instructions`; ignorar `notifications/*`; responder `ping`, `tools/list`, `tools/call`, `resources/list` y `prompts/list`. Los fallos de herramienta van como resultado `isError: true`, no como excepción que mata el proceso.
3. Configuración de OpenCode v2: `mcp.servers.<nombre>` con `type: "local"`, `command: [bin, script]`, `cwd` y `environment`. Las herramientas se nombran `<servidor>_<tool>`; en Code Mode, `tools.<servidor>.<tool>`. Tras editar la configuración global, `opencode mcp list` puede tardar unos segundos en ver el servidor.
4. Geometría del vaso: cilindro hueco con fondo, 8 triángulos por segmento. Forzar ángulo `0` en la última vuelta: `2*pi` calculado no es exactamente cero y deja aristas sin aparear.
5. Visor: HTTP en `127.0.0.1`, sin CDN, validando `Host` y `Origin`, sirviendo solo el HTML y los `*.stl` de una carpeta acotada. El puerto sale siempre de una variable de entorno leída de la configuración; en ese equipo un puerto concreto daba `EADDRINUSE` sin aparecer en `lsof`, y otros dos funcionaban.
6. Error real del bucle de dibujo: salir con `return` antes de `requestAnimationFrame` deja el lienzo en blanco si la malla aún no llegó. Pedir el siguiente frame siempre y dibujar solo cuando haya malla. Probarlo extrayendo el `<script>` del HTML y evaluándolo con mocks de DOM/WebGL: antes había cero llamadas a `drawArrays`; después, una con 1536 vértices.

## Cómo se comprueba

Cada arista del STL aparece exactamente dos veces; el archivo binario mide `84 + 50*triangulos` bytes; `opencode mcp list` muestra las herramientas conectadas; el visor responde solo en loopback y dibuja la malla tras la carga. La apariencia visual la confirma la persona.

## Lo que no se ha verificado

La APK de CAD móvil con motor FreeCAD y puente Python no se investigó; el visor WebGL se sustituyó por un visor 2D verificado visualmente en esa instalación.
