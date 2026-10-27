# PM-U5 - Contenidos multimedia

Note: Unidad 5 en un deck. Todo lo que se mide aquí se midió de verdad con ffmpeg 6.1.1 en el aula: los tamaños no son de una tabla de internet, son de ficheros generados delante de los alumnos.

---

## Índice de la unidad

Note: Primero los formatos (qué es un códec), después capturar con la cámara, luego reproducir y por último animar. Es el orden en el que se construye la práctica.

- **5.1** Formatos y entornos multimedia
- **5.2** Captura y procesamiento en Android (CameraX)
- **5.3** Reproducción de audio y vídeo (Media3)
- **5.4** Animaciones

---

## 5.1 · Contenedor y códec: la caja y cómo se guarda

Note: La confusión número uno: MP4 no es un códec, es una caja. Dentro puede llevar H.264 o H.265. Un fichero .mp4 con un códec que el móvil no soporta no se reproduce aunque la extensión sea la correcta.

| Término | Qué es | Ejemplo |
|---|---|---|
| **Contenedor** | la caja que agrupa pistas | `.mp4`, `.webm`, `.mkv` |
| **Códec** | cómo se comprime cada pista | H.264, VP9, AAC, Opus |
| **Bitrate** | datos por segundo | 1,5 Mbps |
| **Resolución** | píxeles de ancho × alto | 1280 × 720 |

- `.mp4` **no** es un códec: es un contenedor
- Mirar lo que hay dentro se hace con `ffprobe`, no con la extensión

---

## 5.1 · Imagen: PNG, JPEG y WebP (medido en clase)

Note: Los tamaños son reales, del mismo dibujo plano exportado en tres formatos. La lección: JPEG es peor para imagen plana (4,5 veces más pesado) y WebP gana en las dos cosas.

| Formato | Tamaño real | Cuándo usarlo |
|---|---|---|
| PNG | 3.010 B | imagen plana, transparencia, sin pérdida |
| JPEG | 13.742 B | **fotos** con muchos tonos |
| WebP | 2.558 B | mejor compresión y transparencia |

- Para una imagen **plana**, JPEG pesa **4,5 veces más** que PNG
- PNG y WebP admiten transparencia; JPEG no

---

## 5.1 · Audio: WAV, MP3, OGG

Note: WAV no comprime: es el más grande con diferencia. OGG/Vorbis es el que mejor relación calidad-tamaño da en este ejemplo. En Android, OGG y MP3 son seguros; WAV solo para sonidos cortos.

| Formato | Tamaño real (3 s) | Compresión |
|---|---|---|
| WAV (PCM) | 264.644 B | ninguna |
| MP3 | 73.395 B | con pérdida |
| OGG (Vorbis) | 9.042 B | con pérdida, muy eficiente |

- WAV = **sin comprimir**; para efectos cortos, no para música
- Un mismo audio en OGG puede pesar **29 veces menos** que en WAV

---

## 5.1 · Vídeo: MP4/H.264 y WebM/VP9

Note: El caso del vídeo enseña que un buen códec con malos ajustes pierde: el WebM salió 3 veces más grande que el MP4 por usar ajustes de tiempo real. La lección no es "VP9 es peor", es "los ajustes mandan".

| Formato | Tamaño real | Nota |
|---|---|---|
| MP4 (H.264 + AAC) | 52.065 B | el más compatible |
| WebM (VP9 + Opus) | 162.215 B | **3× más** por ajustes rápidos |

- El códec no lo es todo: `-deadline realtime` sacrifica tamaño por velocidad
- H.264 es el más compatible en Android y navegador

---

## 5.1 · Mirar y convertir con ffmpeg

Note: ffprobe dice qué hay dentro; ffmpeg convierte. Son las dos herramientas del taller y se usan en la práctica para justificar cada formato elegido.

```bash
# ver qué hay dentro de un fichero (contenedor, códecs, tamaño)
ffprobe -v error -show_entries stream=codec_name,width,height -of default video.mp4

# convertir a WebM con VP9 y Opus
ffmpeg -i video.mp4 -c:v libvpx-vp9 -c:a libopus video.webm

# recortar los primeros 0,15 s de un audio
ffmpeg -i audio.wav -t 0.15 sonido.wav
```

- `ffprobe` = **medir**; `ffmpeg` = **convertir**
- Siempre se mide el resultado: no se supone que ha mejorado

---

## 5.2 · Capturar en Android: las tres piezas de CameraX

Note: CameraX son tres dependencias que se reparten el trabajo. Si falta una, el proyecto no compila. Versiones reales usadas en el aula: 1.6.2.

| Dependencia | Para qué |
|---|---|
| `camera-camera2` | acceso al hardware |
| `camera-lifecycle` | atar la cámara al ciclo de vida |
| `camera-view` | la vista `PreviewView` |

- El permiso `CAMERA` se pide en **ejecución**, no solo en el Manifest
- Sin `camera-lifecycle`, la cámara sigue abierta al salir de la pantalla

---

## 5.2 · Del permiso a la foto, sin salir de Compose

Note: El permiso se pide con rememberLauncherForActivityResult. El resultado se guarda en estado y la UI reacciona. La foto se guarda en filesDir, que es privado de la app.

```kotlin
val pedirPermiso = rememberLauncherForActivityResult(   // lanzador del diálogo
    ActivityResultContracts.RequestPermission()
) { concedido -> permiso = concedido }                  // guarda la respuesta

Button(onClick = { pedirPermiso.launch(Manifest.permission.CAMERA) }) {
    Text("Dar permiso")
}
```

- `filesDir` = almacenamiento **privado** de la app (no hace falta permiso)
- La captura se hace con `ImageCapture` y se guarda como JPEG

---

## 5.2 · Procesar el Bitmap: escala de grises

Note: Se lee el JPEG a Bitmap, se recorren los píxeles y se aplica la luminancia. Los coeficientes 0,299 / 0,587 / 0,114 no son inventados: son los del estándar. Después se reduce y se guarda en PNG.

```kotlin
// luminancia estándar: el ojo es más sensible al verde
val gris = (0.299 * rojo + 0.587 * verde + 0.114 * azul).toInt()
bitmapSalida.setPixel(x, y, Color.rgb(gris, gris, gris))
```

- La media de R, G y B **no** es la escala de grises correcta
- Coeficientes: **0,299 / 0,587 / 0,114** (el verde pesa más)
- Se procesa una **copia reducida** para no bloquear la interfaz

---

## 5.2 · Una trampa real de este taller

Note: PreviewView en modo SurfaceView no sale en las capturas de pantalla: la vista se dibuja en otra capa. La solución es el modo COMPATIBLE (TextureView). Es un fallo que se ve en el examen si no se ha probado.

- `PreviewView` por defecto usa **SurfaceView**: no aparece en `screencap`
- Solución: `implementationMode = ImplementationMode.COMPATIBLE`
- Lo que no se prueba en el dispositivo no está terminado

---

## 5.3 · Media3: el reproductor recomendado

Note: ExoPlayer es el reproductor de Google; Media3 es su paquete actual. Versiones reales del aula: media3-exoplayer y media3-ui 1.11.1. Se usa un PlayerView dentro de AndroidView.

| Pieza | Para qué |
|---|---|
| `media3-exoplayer` | el motor de reproducción |
| `media3-ui` | la vista `PlayerView` (controles) |

- El reproductor tiene **estados**: idle, buffering, ready, ended
- Se escucha con `Player.Listener` (`onPlaybackStateChanged`, `onPlayerError`)

---

## 5.3 · Ciclo de vida: crear y liberar

Note: El reproductor ocupa recursos del sistema. Si no se libera al salir, sigue sonando o consume batería. Se crea y se libera con DisposableEffect.

```kotlin
val reproductor = remember { ExoPlayer.Builder(contexto).build() }  // se crea una vez

DisposableEffect(Unit) {
    onDispose { reproductor.release() }    // se libera al salir de la pantalla
}
```

- `release()` es **obligatorio**: sin él, el vídeo sigue vivo
- Un reproductor por pantalla, no uno por cada recomposición

---

## 5.3 · Errores de reproducción

Note: El error real capturado en clase fue ERROR_CODE_IO_FILE_NOT_FOUND al pedir un fichero que no existe. Se muestra el código en pantalla: un error sin código no se puede depurar.

- `onPlayerError` da un `PlaybackException` con **código**
- `ERROR_CODE_IO_FILE_NOT_FOUND` = la ruta no existe
- El mensaje al usuario debe ser claro; el código, para el desarrollador

---

## 5.4 · Las dos familias de animación

Note: En Compose hay dos maneras: por fotogramas (una secuencia de imágenes) y por propiedades (el sistema interpola entre valores). La primera es para sprites; la segunda, para todo lo demás.

| Familia | Cómo funciona | Nodo / API |
|---|---|---|
| **Por fotogramas** | se cambia la imagen cada X ms | `LaunchedEffect` + `delay` |
| **Por propiedades** | el sistema interpola valores | `animateFloatAsState`, `rememberInfiniteTransition` |

- Por fotogramas = **sprites** (4 dibujos que se alternan)
- Por propiedades = escala, rotación, opacidad con `graphicsLayer`

---

## Resumen exprés de la unidad

Note: Cinco frases. La primera es la que más se falla en el examen.

- **Contenedor ≠ códec**: `.mp4` es una caja, dentro va H.264
- Los tamaños se **miden** con `ffprobe`, no se suponen
- JPEG es malo para imagen plana; OGG ahorra frente a WAV
- CameraX son **tres** dependencias; el permiso se pide en ejecución
- Media3 se **libera** con `release()` al salir

---

## Qué entra en la práctica

Note: Avisar el primer día.

- Tabla de formatos **medida** con `ffprobe` y justificación de cada elección
- Captura con cámara, **permiso en ejecución** y fichero guardado
- Procesado del bitmap (gris o reducción) con medidas de tamaño
- Reproducción de audio y vídeo con **estados** y **error controlado**
- Una animación por fotogramas y otra por propiedades
