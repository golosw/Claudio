# Receta aportada — instalar el kit en OpenCode v2 / Termux

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux 0.118.3 · OpenCode v2.0.31 · 2026-11-18, actualizado a v3.4 el 2026-11-20 (reescrito y anonimizado) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Instalar y actualizar el kit en un equipo sin `git`, sin escritorio y con rutas de Android, sin copiar reglas de escritorio ni fiarse de sintaxis de v1.

## Pasos

1. Descargar siempre la etiqueta publicada, nunca `main`: `curl -fsSL https://codeload.github.com/golosw/Claudio/tar.gz/refs/tags/<etiqueta> | tar xz --strip-components=1` en una carpeta nueva de `<CONFIG>/kit/<etiqueta>/`.
2. Sin `git`, usar `curl` + `tar` para descargar y `diff` para comparar manifiestos.
3. El adaptador `adaptadores/opencode.md` del kit está marcado como sin probar: verificar rutas y claves contra la documentación oficial de OpenCode v2 antes de fiarse. La línea de estado por plugin sigue sin verificar.
4. La sintaxis de compactado de v1 (`preserve_recent_tokens`, `reserved`, `tail_turns`, `prune`) no sirve: en v2 son `compaction.keep.tokens` y `compaction.buffer` (porcentaje o tokens). No se puede fijar un umbral absoluto de 150 000 tokens.
5. `opencode run --standalone --format json "…"` funciona y devuelve eventos con coste y tokens: sirve para verificar sin abrir la TUI.
6. Los cambios en instrucciones, agentes y skills no aparecen en la sesión en curso: hace falta sesión nueva o `opencode reload`.
7. El ejecutable puede estar en una ruta fija del sistema, con lo que no hace falta lanzador propio. En entorno limpio (`env -i`), Termux necesita `TMPDIR=$PREFIX/tmp`; sin él OpenCode intenta crear `/data/local/tmp/opencode` y falla con `EACCES`.
8. Podar de la plantilla las reglas de escritorio (sudo gráfico, systemd, KVM, aplicaciones GUI) antes de instalar: cada sesión paga ese texto.
9. Al actualizar, comparar el manifiesto nuevo con `<CONFIG>/kit/VERSION_INSTALADA/MANIFIESTO.md`, hacer copia verificada antes de tocar nada y volver a leer el archivo de ajustes real: en esta instalación era `opencode.json` y el progreso decía otro nombre y otros valores.

## Cómo se comprueba

`opencode --version`; `opencode run --standalone --format json "¿cuál es tu marca de versión?"` devuelve la marca de la plantilla instalada; `opencode debug agents` y `opencode debug config` sin error; `head -1 <CONFIG>/AGENTS.md` con la marca esperada.

## Lo que no se ha verificado

El SO exacto del equipo donde se hizo la instalación original: el registro previo y la detección actual no coinciden. La versión del kit y del asistente sí están comprobadas.
