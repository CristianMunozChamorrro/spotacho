# SpoTacho 🎵

Reproductor de música para iPhone hecho como web app (PWA). No pasa por la App Store y no caduca.

## Funcionalidades
- **Buscador** de canciones con la API pública de [Audius](https://docs.audius.org). Toca una canción para escucharla al instante, o pulsa **+** para añadirla a una playlist.
- **Playlists ilimitadas**. Cada una nueva se crea con un nombre por defecto («Mi playlist nº N») y luego se puede renombrar o eliminar.
- **Reproducción aleatoria** (botón naranja) o de una canción concreta (tocándola). También tiene modos aleatorio y repetir.
- **Descarga offline**: el audio se guarda en IndexedDB y la playlist suena sin conexión.
- **Mis archivos**: para importar tus propios MP3/M4A desde la app Archivos o iCloud Drive. Lee las etiquetas ID3 para sacar título y artista.
- **Segundo plano y pantalla bloqueada**: la reproducción sigue con controles en la pantalla de bloqueo gracias a la Media Session API.
- **Dynamic Island**: muestra la canción que está sonando. Al tocarla, la app se abre directamente en el reproductor.

## Tecnologías
HTML, CSS y JavaScript sin frameworks · Service Worker · Web App Manifest · IndexedDB · Media Session API · Fetch API (REST).

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

## Limitaciones conocidas (por ser una PWA)
- En la Dynamic Island aparece el reproductor estándar de iOS. Para una Live Activity personalizada haría falta una app nativa en Swift.
- Si iOS se queda sin espacio, puede borrar los datos de una web app que lleve semanas sin abrirse. La app pide almacenamiento persistente para reducir ese riesgo.
- El catálogo es el de Audius: música independiente con licencia para streaming. No incluye artistas de sellos comerciales.
