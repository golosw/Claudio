# Receta aportada — personalidades con AGENTS.md local

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · OpenCode v2.0.31 · 2026-11-18 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Tener roles auxiliares con identidad y método propios, sin crear un subagente por cada rol ni cargar su texto en todas las sesiones.

## Pasos

1. Cada personalidad es una carpeta en `{{PROYECTOS}}/personalidades/` con su propio `AGENTS.md`; no es un subagente.
2. OpenCode carga el `AGENTS.md` global y los de cada carpeta desde el directorio de trabajo hacia casa. Para que se cargue el de proyectos, la sesión debe abrirse dentro de `{{PROYECTOS}}` o por debajo.
3. La regla de activación vive en `{{PROYECTOS}}/AGENTS.md`: resolver el nombre sin tildes ni mayúsculas, listar carpetas si no existe, leer los `.md`, adoptar el papel, volver cuando se pida y actualizar el `ESTADO.md` de la personalidad.
4. Si la sesión se abre desde otro directorio, la personalidad no se carga sola: hay que leer su `AGENTS.md` explícitamente.

## Cómo se comprueba

Dentro de `{{PROYECTOS}}`: `opencode run --standalone --format json "hablar con <rol>: ¿quién eres y cómo trabajas?"` devuelve la identidad y el método del rol; con un nombre inexistente, lista solo las carpetas disponibles sin inventar ninguna.

## Lo que no se ha verificado

Carga automática desde directorios fuera de `{{PROYECTOS}}` (se sabe que no ocurre) y carga en sesiones ya abiertas sin recarga.
