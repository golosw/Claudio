# promptinicial.md — Claudio v3.5

Punto de entrada. Abre tu asistente de terminal en la carpeta donde está este archivo y dile:

> Lee `promptinicial.md` y síguelo.

Sirve para dos casos:
- **Equipo virgen**: construye todo el sistema desde cero, adaptado a quien lo usa.
- **Instalación previa** (cualquier versión anterior o hecha a mano): **actualiza sin destruir**.

Este kit no lleva código. Cada apartado describe **qué** hay que construir, **por qué** y **cómo
se comprueba** que funciona; el código lo escribes tú en la máquina de destino, adaptado a lo
que detectes en ella. Dos instalaciones pueden quedar distintas por dentro: lo que tiene que
coincidir es el comportamiento que piden los criterios de aceptación.

Funciona con varios asistentes de terminal (Claude Code, Codex CLI, Gemini CLI, opencode…). **Se
recomienda Claude Code**: es el de referencia y el único probado de principio a fin. Con los demás,
lo que el programa no permita se degrada de forma visible y el informe final lo dice.

---

## Para el asistente que lee esto

Vas a montar (o actualizar) para el usuario un asistente orquestador personal persistente: vive
en esta máquina, tiene memoria propia que aprende con el uso, delega en subagentes especializados
y se crea herramientas nuevas (agentes y skills) cuando las necesita. Es infraestructura, no una
tarea puntual: hazlo bien y verifícalo.

**Principio que manda sobre todo lo demás: no destruyas trabajo existente.** La máquina puede
tener ya instrucciones, notas de memoria, agentes, skills, hooks y proyectos creados con el
tiempo. Nunca sobrescribas, vacíes ni borres nada de eso. Añade lo que falta, propón las mejoras
como cambios concretos y aplícalos solo tras confirmación. Si dudas de si algo es trabajo del
usuario, trátalo como si lo fuera.

### Cómo se lee el kit

1. Averigua qué asistente de terminal eres y carga su adaptador: lee `adaptadores/README.md` y después
   `adaptadores/<programa>.md` (o aplica el adaptador genérico). Las fases hablan en **términos** entre
   ángulos (`<CONFIG>`, `<INSTRUCCIONES>`, `<AJUSTES>`…) que el adaptador traduce a rutas y órdenes
   reales. Si el programa no es Claude Code, recomiéndalo una vez, en una línea, y sigue sin insistir.
2. Lee `MANIFIESTO.md`: lista cada apartado, su archivo, su versión y si es obligatorio.
3. Sigue las fases **en orden**, leyendo **solo el archivo de la fase en curso** (no cargues todo
   el kit de golpe: gasta contexto y se mezclan instrucciones).
4. Los módulos opcionales (`modulos/`) se leen **solo si el usuario los elige** en la fase 6.
5. Las rutas de este kit son relativas a la carpeta donde está este archivo.
6. Los `{{MARCADORES}}` se rellenan con lo que detectes o preguntes, y los `<TÉRMINOS>` con el
   adaptador; en los archivos finales no puede quedar ninguno de los dos.

### Progreso persistente

Desde el primer paso, lleva el progreso en `<CONFIG>/instalacion-claudio.md`: versión del kit,
identificador del equipo (nombre de host y modelo, sin datos personales), asistente y adaptador, tipo de instalación, datos del usuario ya recogidos, y una lista con cada
fase y módulo marcado como `pendiente`, `hecho (evidencia: …)` o `omitido (motivo)`. Actualízalo al terminar cada fase.
Si la sesión se corta o se compacta, al volver léelo y sigue por donde iba sin repetir preguntas.
Si al empezar ya existe y es de una instalación a medias, ofrece continuarla.

### Orden

| Fase | Archivo | Qué hace |
|---|---|---|
| 0 | `fases/00-detectar.md` | Detectar asistente, máquina e instalación previa (solo lectura) |
| 1 | `fases/01-entrevista.md` | Datos del usuario |
| 2 | `fases/02-copia-seguridad.md` | Copia verificada antes de tocar nada |
| 3 | `fases/03-instrucciones.md` | `<INSTRUCCIONES>` desde `plantillas/instrucciones.md` |
| 4 | `fases/04-memoria-agentes-skills.md` | Vault de memoria, agentes y skills base |
| 5 | `fases/05-ajustes.md` | `<AJUSTES>`, línea de estado, compactado, modelo |
| 6 | `fases/06-modulos.md` | Elegir e instalar módulos; ejecutable estable y accesos directos |
| 7 | `fases/07-verificacion.md` | Verificación real |
| 8 | `fases/08-informe.md` | Informe final |
| 9 | `fases/09-retorno.md` | Informe de retorno para mejorar el kit (opcional, se ofrece) |

Actualizaciones posteriores desde el repositorio: `actualizar.md` (lo instala la fase 4 como
skill; no hace falta leerlo en una instalación).

### Reglas durante la instalación

- Pregunta solo lo que no puedas deducir, con opciones concretas y la recomendada marcada.
- Todo cambio sobre algo que ya existe: diff a la vista y confirmación. Lo nuevo puede ir resumido.
- Marca cada cambio real de fase con una frase corta: es una señal de vida, no narración.
- Nada está hecho sin prueba real: cada apartado tiene sus criterios de aceptación y la fase 7 los
  repasa todos contra el `MANIFIESTO`.
