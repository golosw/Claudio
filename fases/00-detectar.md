# Fase 0 — Detectar (solo lectura, no crees ni cambies nada todavía)

0. **Asistente**: identifica qué programa te está ejecutando y carga su adaptador, según
   `adaptadores/README.md`. A partir de aquí, los términos entre ángulos (`<CONFIG>`,
   `<INSTRUCCIONES>`, `<CLI>`…) tienen el valor de ese adaptador. Anota el programa, su versión
   (`<CLI> --version`) y el adaptador en el progreso.
1. **Máquina**: SO y versión, entorno de escritorio y sesión gráfica (o ninguna), RAM,
   CPU, GPU, disco libre, runtimes disponibles (python, node, gestor de paquetes) y si
   admite máquinas virtuales (KVM accesible para el usuario) y contenedores.
   Añade una salud básica que sirve para recomendar módulos: si el SO sigue con soporte y
   si hay actualizaciones o un reinicio pendientes. Las carpetas de usuario (escritorio,
   documentos) se resuelven con el mecanismo del sistema (`xdg-user-dir` en Linux, rutas
   estándar en macOS/Windows), nunca asumiendo un nombre como `~/Desktop`.
   **Perfil de plataforma**: con lo anterior, clasifica el equipo en uno de estos perfiles; las fases,
   los módulos y la plantilla de instrucciones dicen qué aplica en cada uno:
   - `linux`: Linux con sesión gráfica;
   - `macos`;
   - `windows` (anota también la shell del asistente: PowerShell 5.1/7, Git Bash, WSL);
   - `terminal`: sin escritorio ni sesión gráfica: Android/Termux, servidor por SSH, contenedor o
     Linux sin sesión gráfica. Aquí no hay `xdg-user-dir`, lanzadores, ventanas ni, a menudo,
     systemd, root o KVM: no los busques como fallo; anótalos como «no aplica».
   Si dudas entre dos (p. ej. WSL dentro de Windows, o un Linux con escritorio al que se entra por
   SSH), usa el de la sesión donde corre el asistente y dilo en el resumen.
   **Equipo gestionado**: mira si lo administra una organización (Windows: `dsregcmd /status`
   unido a dominio o Entra/MDM; macOS: `profiles status -type enrollment`; Linux: `realm list` o
   agentes de gestión conocidos). Si lo es, anótalo: los servicios, tareas y políticas corporativas no
   se tocan, y lo que necesite administrador se agrupa en un solo script (M5).
2. **Estado de `<CONFIG>`**: para cada una de estas piezas, anota si existe y cuánto
   contiene: `<INSTRUCCIONES>`, `<AJUSTES>` (lo que el adaptador diga que guarda: entorno, hooks,
   permisos, línea de estado, compactado), `<AGENTES>`, `<SKILLS>`, `memoria/` (notas por carpeta e
   `INDICE.md`), `hooks/`. Mira también si hay carpetas de proyectos con `ESTADO.md` o
   `DECISIONES.md` y si hay un `<INSTR_LOCAL>` en la carpeta de trabajo actual. Si en la carpeta de
   configuración de **otro** asistente hay una instalación de este kit (su `instalacion-claudio.md` o
   la marca en sus instrucciones), anótalo: en la Fase 4 se ofrece reutilizar su `memoria/`. Detecta también, en la carpeta de
   proyectos, una interfaz ya instalada (servicio de usuario que sirve una página en
   127.0.0.1 y lanza el asistente, sus lanzadores o autoarranque y su carpeta de datos) y una
   carpeta `personalidades/`. Si existe `<CONFIG>/instalacion-claudio.md`, léelo: dice qué
   versión del kit se instaló, qué módulos y si quedó algo a medias. Compara su identificador de equipo
   con el de esta máquina: si no coincide, el registro es de **otro equipo** (carpeta copiada o
   sincronizada): dilo, no lo des por válido ni lo sobrescribas, y pregunta si esta máquina es una
   instalación nueva o el mismo equipo renombrado.
   **Carpeta de proyectos (`{{PROYECTOS}}`)**: es la carpeta raíz donde viven mis proyectos, su
   `personalidades/` y el `<INSTR_LOCAL>` local; si dentro hay una subcarpeta `projects/` o
   `proyectos/`, los proyectos van en ella. En una instalación previa **usa la que ya se
   estaba usando y no crees una nueva** (ni en Documentos ni en ninguna otra parte). Búscala
   por estas pistas, de más a menos fiable: la carpeta de trabajo actual si tiene `<INSTR_LOCAL>`,
   `personalidades/` o proyectos con `ESTADO.md`; la carpeta raíz configurada en una interfaz ya instalada; las rutas que citan `<INSTRUCCIONES>`, `memoria/` y los servicios o
   lanzadores; y las carpetas de trabajo que aparezcan en `<SESIONES>`, si el adaptador las da. Si las pistas apuntan a varias, pregúntame cuál con las candidatas como
   opciones; si no hay ninguna pista, se trata como equipo virgen.
3. **Clasifica la instalación**:
   - **Virgen**: no hay `<INSTRUCCIONES>` ni `memoria/`.
   - **Previa**: hay configuración, pero `<INSTRUCCIONES>` no empieza con la marca
     `<!-- prompt-inicial v`.
   - **Con marca**: la marca existe. Solo revisarás las piezas que falten y los módulos. Si su
     versión es anterior a la de `plantillas/instrucciones.md` (p. ej. `v2.7 ·` frente a `v3.0 ·`), las
     reglas y secciones nuevas de la plantilla cuentan como piezas que faltan (fusión de la Fase 3).
     Las instalaciones v2.x traían código de referencia ya copiado (interfaz, hooks, scripts):
     es trabajo instalado y se conserva; la v3 no lo sustituye por nada. **Excepción: los canales de
     actualización.** Si una interfaz u otra pieza heredada busca versiones nuevas en un servidor
     propio (ajuste de servidor de actualizaciones, `version.json`, `kit.json`, carpeta `uiupdate`),
     márcala en el resumen como «a adaptar»: en la Fase 6 se aplica lo que dice M4 para interfaces
     heredadas.
4. Enséñame un resumen corto (una tabla: pieza, estado, qué harás con ella; incluye el perfil de
   plataforma, si es un equipo gestionado, el asistente y su adaptador, las capacidades que quedan degradadas, y la carpeta de proyectos detectada y con
   qué pista) antes de seguir.
