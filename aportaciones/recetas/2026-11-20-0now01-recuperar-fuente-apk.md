# Receta aportada — recuperar y recompilar el fuente de una APK

| Dato | Valor |
|---|---|
| Tipo | receta |
| Probado en | Android/Termux · 2026-11-18 (reescrito y anonimizado 2026-11-20) |
| Perfil de plataforma | terminal |

## Problema u objetivo

Recuperar el código de una APK propia cuyo fuente se perdió, y volver a compilarla verificando que solo cambió lo reescrito.

## Pasos

1. Instalar `jadx` y `apktool` con `pkg`; `TMPDIR` debe apuntar a `$PREFIX/tmp` porque `/tmp` no es escribible.
2. `jadx -d salida --no-res --no-imports <app>.apk` da el Java; `apktool d -f -o salida <app>.apk` da manifiesto legible, `res/`, `assets/` y `smali/`.
3. jadx no garantiza Java compilable. Correcciones típicas: quitar `final` del `for` y copiar el índice a una variable `final`; quitar `throws` en `Runnable`; reescribir `catch` con reasignación como `try/catch/finally`; declarar no `final` una variable asignada en `try` y `catch` y copiarla a una `final` antes del lambda.
4. Los enteros de recursos se recuperan en `R.java` y coinciden con `res/values/public.xml`.
5. El manifiesto de apktool trae `compileSdkVersion`/`platformBuildVersion*`: quitarlos antes de `aapt2 link` o pasar `--min-sdk-version`/`--target-sdk-version`.
6. `aapt2 link` exige recursos compilados: `aapt2 compile --dir res -o build/res` y pasar los `.flat`. `-A assets` incluye los assets.
7. Comparar con el original: `cmp` de los assets extraídos, `aapt2 dump badging` y nombres de clase con `unzip -p <app>.apk classes.dex | grep -ao 'Lcom/...'`. `strings` no está disponible.
8. Pasar jadx a la APK nueva y hacer `diff` contra la vieja para confirmar que solo cambió lo reescrito.

## Cómo se comprueba

La APK regenerada pasa `apksigner verify`, conserva los mismos assets y clases y el `diff` de Java descompilado solo muestra los cambios introducidos.

## Lo que no se ha verificado

Instalación y uso de la APK regenerada en el dispositivo; la recuperación de código ofuscado no se intentó.
