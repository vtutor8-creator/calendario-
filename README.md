# 🎄 Calendario de Adviento

Un calendario de Adviento para regalar: cada día de diciembre (del 1 al 24) se
abre con una canción (reproductor de YouTube dentro de la app), un libro de
dominio público para leer gratis, una frase cariñosa y un plan del día.
El día 24 tiene un mensaje final y el 25 (Navidad) se desbloquea un botón
sorpresa que exporta la playlist completa (abrir en YouTube, copiar o compartir).

## Estructura

```
www/index.html      ← la app (versión final, con candados por fecha)
www/prueba.html     ← versión de prueba (todos los días abiertos)
capacitor.config.json
package.json
.github/workflows/android.yml   ← genera el APK en GitHub
```

## Personalizar (en `www/index.html`)

Busca estas líneas y edítalas:

- `const NOMBRE='',DE='';` → pon su nombre y el tuyo, por ejemplo
  `const NOMBRE='Ana',DE='Luis';`
- `const MEDIA={...}` → los ID de YouTube de cada día (`1:{yt:'ID'}`).
  También acepta archivos propios: `1:{src:'audio/01.mp3'}`.
- `const SONGS=[...]` → título y artista que se muestran.
- `const BK2=[...]` → los libros (título, autor, motivo, búsqueda).
- `const F=[...]` → las 24 frases del día.
- `const D=[...]` → emoji y "plan del día" de cada casilla.

Los candados siguen las fechas reales (1–24 de diciembre). Para ver todo
abierto: abre la app con `#probar` al final de la dirección, o usa
`www/prueba.html`.

## Probar en el navegador

```bash
npm install
npm run serve        # abre http://localhost:3000 (también /prueba.html)
```

No abras el archivo con doble clic: YouTube suele dar error al incrustar
desde `file://`. Usa el servidor local.

## Generar el APK de Android (sin instalar nada)

1. Sube esta carpeta a un repositorio de GitHub (rama `main`).
2. Ve a **Actions → Construir APK** (se ejecuta solo al hacer push).
3. Al terminar, descarga `calendario-adviento-apk` en *Artifacts*.
4. Pasa el `app-debug.apk` al teléfono e instálalo (permite "orígenes
   desconocidos").

## Android / iOS en tu computadora

```bash
npm install
npx cap add android      # y/o: npx cap add ios  (iOS requiere Mac + Xcode)
npx cap sync
npx cap open android     # o: npx cap open ios
```

Cada vez que cambies `www/`, ejecuta `npx cap sync`.

## Notificaciones diarias

La app programa una notificación local por día (1 al 25 de diciembre) a las
9:00. La hora se cambia en `www/index.html` con `const HORA=9;`.

- La primera vez que se abre, el teléfono pide permiso para notificar.
- No necesita internet ni servidor: las programa el propio teléfono.
- Para probarlas: copia `www/prueba.html` sobre `www/index.html`, genera la
  app y ábrela; a los 10 segundos llega una notificación de prueba.
  (Después vuelve a poner la versión final.)
- En algunos Android el ahorro de batería puede retrasarlas unos minutos.

## Notas

- Si un video no se puede incrustar (algunos son de discográficas),
  la tarjeta muestra un enlace "Ábrelo en YouTube".
- Las fuentes se cargan de Google Fonts; sin internet se usa una tipografía
  del sistema.
- Los libros enlazan a Standard Ebooks y Wikisource (dominio público).
- El APK es de depuración, ideal para un regalo. Para Google Play hay que
  firmarlo y generar un AAB.
