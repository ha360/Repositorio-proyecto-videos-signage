# Reproductor de Signage (Samsung Tizen – URL Launcher)

Pantalla negra a pantalla completa que reproduce tus videos en bucle, leyéndolos
directamente desde un repositorio de GitHub. Sin servidor, sin MagicINFO, sin costo.

---

## 1. Montaje inicial (una sola vez)

1. Crea un repositorio **público** en GitHub, por ejemplo `signage`.
2. Sube `index.html` a la **raíz** del repositorio.
3. Crea una carpeta llamada `videos` y sube ahí tus archivos `.mp4`
   (en GitHub web: *Add file → Upload files →* arrastras y sueltas).
4. Abre `index.html`, edita el bloque **CONFIG** (solo 2 líneas obligatorias):
   ```js
   GITHUB_USER: "tu_usuario",
   GITHUB_REPO: "signage",
   ```
5. Activa **GitHub Pages**: *Settings → Pages → Build and deployment →*
   *Branch: `main` / carpeta `/ (root)` → Save.*
   GitHub te dará una URL como:  `https://tu_usuario.github.io/signage/`

## 2. Configurar la pantalla

1. En el control: botón **Home**.
2. Cambia **Play via** (o *App Management* en Tizen 7+) a **URL Launcher / Custom App**.
   - Si no ves "URL Launcher", la pantalla está en modo **MagicINFO**; ahí mismo
     se cambia a URL Launcher.
3. Escribe la URL de GitHub Pages. **Todo en minúsculas**, incluida la `h` de `https`.
   - Si la red bloquea HTTPS, prueba con `http`.
4. Verifica **Fecha y hora** correctas (*System → Time*). Si están mal, no conecta.
5. Guarda y reinicia. La pantalla cargará la página y empezará a reproducir.

---

## 3. Manejo diario

- **Agregar un video:** súbelo a la carpeta `videos`. Aparece solo (se revisa cada 20 min,
  o al reiniciar la pantalla).
- **Cambiar el orden:** crea/edita el archivo `videos/playlist.json` con los nombres
  en el orden deseado:
  ```json
  [
    "01-intro.mp4",
    "promocion-marzo.mp4",
    "seguridad-bodega.mp4"
  ]
  ```
  - Lo que esté en la lista va primero, en ese orden.
  - Lo que no esté en la lista se reproduce después, en orden alfabético.
  - Si no existe `playlist.json`, todo va en orden alfabético
    (por eso puedes nombrarlos `01-...`, `02-...`).
- **Quitar un video:** bórralo de la carpeta `videos`.

---

## 4. Ajustes opcionales (bloque CONFIG en index.html)

| Opción        | Valores                        | Para qué sirve                                  |
|---------------|--------------------------------|-------------------------------------------------|
| `MUTED`       | `true` / `false`               | `true` = sin sonido (recomendado para autoplay) |
| `FIT`         | `"contain"` / `"cover"`        | `contain` = ver completo · `cover` = llenar      |
| `REFRESH_MIN` | número de minutos              | cada cuánto revisa si hay videos nuevos          |

> Tocar la pantalla activa/desactiva el sonido en sitio.

---

## 5. Cosas importantes a tener en cuenta

- **Repositorio público:** cualquiera con la URL podría ver los videos.
  No subas material confidencial.
- **Tamaño de archivo:** GitHub no acepta archivos de más de **100 MB**.
  Comprime los videos antes de subirlos. Ejemplo con ffmpeg:
  ```
  ffmpeg -i entrada.mov -c:v libx264 -crf 23 -preset slow \
         -vf "scale=1920:-2" -c:a aac -b:a 128k -movflags +faststart salida.mp4
  ```
- **Formato:** usa **MP4 (H.264)**. Es el que mejor reproduce el navegador de Tizen.
- **Muchas pantallas en la misma red:** la API de GitHub permite ~60 consultas por hora
  por IP pública. Si tienes muchas pantallas, sube `REFRESH_MIN` (p. ej. 30 o 60).
- **Red con portal cautivo (login web):** algunas redes "libres" piden aceptar términos
  en una página; ahí el signage no pasa solo. Usa una red sin portal cautivo.
