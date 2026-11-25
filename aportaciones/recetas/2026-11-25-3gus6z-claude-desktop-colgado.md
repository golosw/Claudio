# Receta aportada — App de escritorio de Claude (MSIX) colgada tras suspensión en Windows

| Dato | Valor |
|---|---|
| Tipo | error (diagnóstico de un fallo) |
| Probado en | Windows 11 Enterprise build 26300 · Claude Desktop 6.6395 (MSIX) junto a Claude Code 2.1.328 · 2026-11-25 |
| Perfil de plataforma | windows |

## Problema u objetivo

La app de escritorio de Claude no responde bien al abrirla. Al lanzarla otra vez no arranca una nueva:
`main.log` solo registra `second-instance: the main window is made; it is shown now` y se reaprovecha la
instancia colgada.

## Causa real

Instancia arrancada tras un cierre no limpio y que después pasó horas en suspensión con un corte de red.
Evidencia en `%LOCALAPPDATA%\Claude\Logs\main.log`:
- bloque `[startup-sign-in] state` con `previous_quit_clean: false` y `sign_in_cookie_present: null`
  (lectura del almacén de cookies de varios segundos);
- `[startup-perf]` con resolución del proxy de decenas de segundos y la ventana creada tarde;
- `claude.ai-web.log` sin escrituras desde la suspensión (la parte web murió).

Además, la posición guardada de la ventana estaba fuera de pantalla (`x` negativa, de un monitor
desconectado); la app la recoloca al arrancar de nuevo.

## Pasos

1. Localizar solo los procesos de la app de escritorio, por ruta, y cerrarlos por PID. **No** usar
   un patrón por nombre: `claude.exe` del CLI (`<usuario>\.local\bin\claude.exe`) tiene el mismo nombre
   y puede estar sosteniendo la sesión actual del asistente.
   ```powershell
   Get-CimInstance Win32_Process -Filter "Name='Claude.exe'" |
     Where-Object ExecutablePath -like 'C:\Program Files\WindowsApps\Claude_*\app\Claude.exe' |
     ForEach-Object { Stop-Process -Id $_.ProcessId -Force }
   ```
2. Lanzarla por su AUMID exacto (el paquete incluye varias aplicaciones: SshAskpass, SshProxy…, así que
   el identificador debe ser el de `Claude`):
   ```powershell
   Start-Process "shell:AppsFolder\Claude_pzs8sxrjxfjjc!Claude"
   ```
   Si falla, obtenerlo con `(Get-StartApps | Where-Object Name -eq 'Claude').AppID`.

Al forzar el cierre, el log registra `Network Service process gone (crashed)`: lo provoca el propio
cierre, no es la causa. Los avisos `EBUSY ... chrome-native-host.exe` son benignos.

## Cómo se comprueba

En el bloque `[startup-sign-in] state` nuevo de `main.log`: `sign_in_cookie_present: true` y la ventana
creada en menos de 1 s; `claude.ai-web.log` vuelve a escribirse con hora actual; el proceso tiene una
ventana con título «Claude» (`(Get-Process -Id <PID>).MainWindowTitle`).

## Lo que no se ha verificado

- Que la ventana se vea y cargue las conversaciones lo confirma la persona a simple vista.
- No se ha reproducido el cuelgue a propósito; la causa se deduce de los logs de un solo caso.
