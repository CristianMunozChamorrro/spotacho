# SpoTacho 🎵

Reproductor de música para iPhone hecho como web app (PWA). No pasa por la App Store y no caduca.

## Funcionalidades
- **Buscador con varias fuentes**, consultadas en paralelo:
  - **YouTube** (YouTube Data API v3 + reproductor oficial IFrame): casi cualquier canción. Se escucha con conexión.
  - **[Audius](https://docs.audius.org)** (API pública): música independiente que se puede descargar.
  - **Tus archivos** importados.

  Cada resultado lleva una etiqueta con su origen. Toca una canción para escucharla al instante, o pulsa **+** para añadirla a una playlist.
- **Playlists ilimitadas**. Cada una nueva se crea con un nombre por defecto («Mi playlist nº N») y luego se puede renombrar o eliminar.
- **Reproducción aleatoria** (botón naranja) o de una canción concreta (tocándola). También tiene modos aleatorio y repetir.
- **Descarga offline**: el audio se guarda en IndexedDB y la playlist suena sin conexión.
- **Mis archivos**: para importar tus propios MP3/M4A desde la app Archivos o iCloud Drive. Lee las etiquetas ID3 para sacar título y artista.
- **Segundo plano y pantalla bloqueada**: la reproducción sigue con controles en la pantalla de bloqueo gracias a la Media Session API.
- **Dynamic Island**: muestra la canción que está sonando. Al tocarla, la app se abre directamente en el reproductor.

## Tecnologías
HTML, CSS y JavaScript sin frameworks · Service Worker · Web App Manifest · IndexedDB · Media Session API · Fetch API (REST).

## Crear la clave de la API de YouTube (gratis, 5 minutos)
1. Entra en https://console.cloud.google.com con tu cuenta de Google.
2. Arriba, en el selector de proyectos, pulsa **Nuevo proyecto**, llámalo `SpoTacho` y créalo.
3. Menú ☰ → **APIs y servicios → Biblioteca**. Busca **YouTube Data API v3** y pulsa **Habilitar**.
4. Menú ☰ → **APIs y servicios → Credenciales → + Crear credenciales → Clave de API**. Copia la clave (empieza por `AIza…`).
5. *(Recomendado)* Pulsa sobre la clave y, en **Restricciones de aplicaciones**, elige **Sitios web** y añade `https://TU_USUARIO.github.io/*`. En **Restricciones de API**, deja solo **YouTube Data API v3**. Así nadie puede usar tu clave desde otra web.
6. En SpoTacho, pulsa ⚙ **Ajustes**, pega la clave y pulsa **Guardar**. La app la comprueba antes de guardarla.

> La cuota gratuita es de 10.000 unidades al día. Cada búsqueda gasta unas 101, así que salen unas 100 búsquedas diarias. La cuota se renueva cada día a las 9:00, hora española. Las búsquedas repetidas se guardan en memoria y no gastan cuota.

## Cómo instalarla en el iPhone (GitHub Pages, gratis)
1. Crea un repositorio en GitHub (por ejemplo `spotacho`) y sube todos los archivos de esta carpeta.
2. Ve a **Settings → Pages**, elige *Deploy from a branch*, rama `main` y carpeta `/ (root)`, y guarda.
3. En un minuto estará en `https://TU_USUARIO.github.io/spotacho/`.
4. Ábrela en **Safari** desde el iPhone y pulsa **Compartir → Añadir a pantalla de inicio**.

> Hace falta HTTPS para el Service Worker, y GitHub Pages ya lo da. Para probarla en el ordenador vale con `python3 -m http.server` y abrir `http://localhost:8000`.

## Estructura
| Archivo | Qué hace |
|---|---|
| `index.html` | Interfaz y toda la lógica (estado, API, reproductor, descargas) |
| `sw.js` | Guarda la app en caché para que abra sin conexión |
| `manifest.webmanifest` | Nombre, icono y modo pantalla completa |
| `icon-*.png`, `apple-touch-icon.png` | Iconos |

## Arquitectura de fuentes (para la memoria del proyecto)
- **Motor de reproducción unificado** (`Engine`): la misma interfaz `play / pause / seek / time` sirve para dos motores distintos. Uno es `<audio>` HTML5, que reproduce Audius, las descargas y los MP3. El otro es `YT.Player`, que reproduce YouTube. Las colas y las playlists pueden mezclar canciones de cualquier origen.
- **Búsqueda en paralelo** con `Promise.allSettled`: si una fuente falla, se muestran los resultados de la otra.
- **Capacidades distintas según la fuente**:

| Fuente | Catálogo | Offline | Pantalla bloqueada |
|---|---|---|---|
| YouTube | Casi todo | No (lo prohíben sus condiciones) | No |
| Audius | Música independiente | Sí | Sí |
| Mis archivos | Lo que importes | Sí | Sí |

## Limitaciones conocidas (por ser una PWA)
- Las canciones de YouTube se reproducen con el reproductor oficial, que tiene que estar visible. Por eso, al poner una se abre el reproductor grande con el vídeo. Se paran al bloquear la pantalla y no se pueden descargar. En algunos iPhone hay que tocar el vídeo la primera vez para que empiece; la app lo avisa.
- En la Dynamic Island aparece el reproductor estándar de iOS. Para una Live Activity personalizada haría falta una app nativa en Swift.
- Si iOS se queda sin espacio, puede borrar los datos de una web app que lleve semanas sin abrirse. La app pide almacenamiento persistente para reducir ese riesgo.
