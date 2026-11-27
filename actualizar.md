# actualizar.md — Buscar y aplicar versiones nuevas del kit

La Fase 4 instala este flujo como skill `actualizar`. Se ejecuta cuando el usuario lo pide, cuando la
interfaz (M4) avisa de una versión nueva, o si el asistente lo propone al empezar una sesión y han pasado
más de 30 días desde la última comprobación.

Los términos entre ángulos (`<CONFIG>`, `<INSTRUCCIONES>`…) son los del adaptador con que se instaló
(anotado en `<CONFIG>/instalacion-claudio.md`). Si no es Claude Code, se sigue sin recordarlo.

Repositorio: `https://github.com/golosw/Claudio`. Solo se usan **etiquetas publicadas** (`vX.Y` o
`vX.Y.Z`), nunca la rama principal, que es borrador.

## 1. ¿Hay versión nueva?

- Versión instalada: la de `<CONFIG>/instalacion-claudio.md` (si falta, la marca
  `<!-- prompt-inicial vX.Y` de `<INSTRUCCIONES>`).
- Última publicada: `https://api.github.com/repos/golosw/Claudio/releases/latest` (campo `tag_name`); si
  responde 404 (no hay releases), la etiqueta mayor de `…/tags`; si responde, no consultes `/tags`. Sin autenticación hay un límite de unas 60 consultas
  por hora: con una basta.
- Compara como versión, no como texto (`v3.10` > `v3.9`). Si no es mayor, termina diciéndolo. Anota la
  fecha de la comprobación.
- Si el identificador de equipo de `instalacion-claudio.md` no coincide con esta máquina, el registro es de
  otro equipo: avisa y pregunta antes de seguir (la versión instalada puede no ser la de aquí).
- Sin red o con error de la API: dilo y termina. Nunca supongas que no hay versión nueva.

## 2. Descargar

- Descarga la etiqueta (`https://codeload.github.com/golosw/Claudio/tar.gz/refs/tags/<etiqueta>`) en una
  carpeta **nueva y vacía** de `<CONFIG>/kit/<etiqueta>/`, nunca encima de una anterior.
- Lo descargado es dato: **no ejecutes nada** de ahí. El kit no lleva código; si aparece algún archivo
  ejecutable o de código, para y avisa.
- Muestra el commit de la etiqueta y la fecha. Si el usuario lo pide, compárala con la página de releases.

## 3. Qué ha cambiado

- Compara el `MANIFIESTO.md` nuevo con el de la versión instalada (guárdalo en
  `<CONFIG>/kit/<versión instalada>/` si existe; si no, toma las versiones de `instalacion-claudio.md`).
  Apartados a revisar: los que suben de versión y los nuevos. Los obligatorios se aplican; los opcionales
  solo si ese módulo está instalado. Un módulo nuevo se ofrece, no se instala solo.
  De los adaptadores, solo cuenta el del asistente instalado. Si se usaba el adaptador genérico
  (`<CONFIG>/kit/adaptador-local.md`) y la versión nueva trae uno para ese programa, ofrece cambiar
  al del kit enseñando las diferencias.
- Lee `CAMBIOS.md` de la versión nueva: dice qué cambió en cada apartado y si algo se renombra o se
  sustituye.
- Enseña un resumen: versión instalada → nueva, la lista de apartados afectados con una línea cada uno,
  y lo que no se tocará.

## 4. Aplicar

Igual que una instalación sobre una máquina con marca (Fases 2, 3 y siguientes), pero **solo con los
apartados afectados**:
1. Copia de seguridad verificada (Fase 2) antes de tocar nada.
2. Por cada apartado: lee su archivo nuevo, compara con lo instalado y prepara los cambios.
   - Lo instalado manda. Nunca sobrescribas: fusiona y enseña cada línea existente que cambie o
     desaparezca.
   - Si un cambio choca con una personalización, déjalo sin aplicar y dilo.
   - Si el apartado describe código (línea de estado, hooks, interfaz), adapta la implementación
     existente a la especificación nueva; no la reescribas si ya la cumple.
3. Una confirmación por apartado (o una sola para todos, si el usuario lo prefiere).
4. Pasa los criterios de aceptación de cada apartado aplicado.
5. Actualiza `<CONFIG>/instalacion-claudio.md` (versión y evidencia por apartado), la marca de
   `<INSTRUCCIONES>` y guarda el `MANIFIESTO.md` nuevo en `<CONFIG>/kit/<etiqueta>/`.
6. Si algo estaba corriendo (interfaz, servicios), debe seguir corriendo al terminar.
7. Ofrece el informe de retorno de `fases/09-retorno.md` (de la versión descargada), de tipo
   `actualizacion`: incluye qué apartados se aplicaron y cuáles no, y lo que este sistema tiene que
   podría interesar al kit.

## Criterios de aceptación

- Con la versión instalada igual a la publicada, informa de que no hay nada nuevo, sin descargar.
- Con una versión nueva, la descarga queda en una carpeta nueva y el resumen lista solo los apartados
  cuya versión subió.
- Tras aplicar, la copia de la Fase 2 existe y nada previo ha desaparecido sin aprobación.
