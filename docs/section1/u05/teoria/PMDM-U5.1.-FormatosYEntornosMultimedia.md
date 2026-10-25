---
title: "Tema 5.1 - Formatos y entornos multimedia"
summary: "Contenedor y códec, imagen, audio y vídeo: qué formato elegir, cómo se mira un fichero con ffprobe y cómo se convierte con ffmpeg, con medidas reales."
series: "Programación de Dispositivos Móviles"
series_order: 5
section: "5. Contenidos multimedia"
author: Ismael Velasco
date: 2026-10-25
weight: 1
---
# Tema 5.1 - Formatos y entornos multimedia

Antes de reproducir nada hay que saber **qué hay dentro de un fichero multimedia**. En este apartado no hay app todavía: hay ficheros reales, medidas reales y conversiones reales hechas con `ffmpeg` en el aula.

<div style="background-color:#a5d6a7; border-radius:8px; padding:10px 14px; margin:14px 0;">
<strong>🟢 Leyenda de repaso obligatorio:</strong> los textos con <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">este fondo verde</span> son imprescindibles y entran en todas las evaluaciones del módulo.</div>

## 1. Los entornos de desarrollo multimedia

Un entorno multimedia es el programa donde **capturas, editas o conviertes** contenido:

| Entorno | Para qué | Dónde se usa |
|---|---|---|
| Android Studio | captura y reproduce dentro de la app | desarrollo de la app |
| `ffmpeg` / `ffprobe` | convertir, recortar y **mirar** cualquier fichero | línea de comandos |
| GIMP / Krita | editar imagen | escritorio |
| Audacity | editar audio | escritorio |
| Kdenlive / DaVinci Resolve | editar vídeo | escritorio |
| ExoPlayer (Media3) | reproducir en la app | dentro del código |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`ffmpeg` es el entorno multimedia más importante para un programador: convierte prácticamente cualquier formato y `ffprobe` te dice qué hay dentro de un fichero.</span>

## 2. Contenedor y códec: la caja y cómo se guarda

- El **contenedor** es la caja: `.mp4`, `.webm`, `.mkv`. Dentro puede haber vídeo, audio y subtítulos.
- El **códec** es cómo se comprime cada pista: `h264`, `vp9`, `aac`, `opus`, `mp3`.

Un `.mp4` no dice nada del códec por sí solo: puede llevar H.264 o H.265 dentro. Por eso hay que mirar el fichero, no la extensión.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Contenedor = caja (`.mp4`, `.webm`). Códec = compresión (`h264`, `vp9`, `aac`). Dos ficheros con la misma extensión pueden llevar códecs distintos.</span>

## 3. Imagen: PNG, JPEG y WebP

| Formato | Compresión | Transparencia | Cuándo usarlo |
|---|---|---|---|
| PNG | sin pérdida | sí | capturas, iconos, zonas planas |
| JPEG | con pérdida | no | fotos |
| WebP | con o sin pérdida | sí | web y apps modernas |

Medido en el aula con la misma imagen de 640 × 360:

| Fichero | Tamaño | Códec |
|---|---|---|
| `imagen.png` | 3.010 bytes | png |
| `imagen.jpg` (calidad 2) | 13.742 bytes | mjpeg |
| `imagen.webp` (calidad 80) | 2.558 bytes | webp |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Convertir no siempre es reducir: en una imagen de zonas planas, el PNG pesó **4,5 veces menos** que el JPEG. El formato se elige por el contenido, no por la moda.</span>

## 4. Audio: WAV, MP3, OGG y AAC

Lo que define un audio:

- **Frecuencia de muestreo**: 44.100 Hz es lo habitual (calidad CD).
- **Canales**: 1 (mono) o 2 (estéreo).
- **Profundidad**: 16 bits es lo normal.
- **Bitrate**: cuántos bits por segundo; a más bitrate, más calidad y más tamaño.

Medido con el mismo tono de 3 segundos:

| Fichero | Tamaño | Códec | Bitrate |
|---|---|---|---|
| `audio.wav` | 264.644 bytes | pcm_s16le | 705.717 bps |
| `audio.mp3` (192k) | 73.395 bytes | mp3 | 193.769 bps |
| `audio.ogg` (calidad 5) | 9.042 bytes | vorbis | 24.112 bps |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El WAV no comprime: guarda la onda tal cual. Pasar a OGG lo dejó en **29 veces menos** sin dejar de ser audible.</span>

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El bitrate es la medida del compromiso calidad/tamaño: 192 kbps en MP3 es calidad alta; 64 kbps se nota.</span>

## 5. Vídeo: MP4/H.264 y WebM/VP9

Un vídeo añade dos datos más: **resolución** (640 × 360) y **fotogramas por segundo** (30 fps).

| Fichero | Tamaño | Vídeo | Audio | Bitrate |
|---|---|---|---|---|
| `video.mp4` | 52.065 bytes | h264 | aac | 138.840 bps |
| `video.webm` | 162.215 bytes | vp9 | opus | 428.856 bps |

Aquí está la lección incómoda: **el WebM salió tres veces más grande**. No es que VP9 sea peor códec: es que se codificó con `-deadline realtime -cpu-used 5`, pensado para ir rápido, no para comprimir bien.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un códec mejor con malos ajustes pierde contra un códec peor bien ajustado. La conversión se mide, no se supone.</span>

## 6. Mirar un fichero: `ffprobe`

`ffprobe` lee el fichero y dice qué contiene, sin reproducirlo:

```bash
ffprobe -v error -show_entries format=format_name,duration,bit_rate \
        -show_entries stream=codec_name,width,height,sample_rate,channels \
        -of default=noprint_wrappers=1 video.mp4
```

Salida real de los ficheros del aula:

```text
imagen.png:   codec_name=png     width=640 height=360
imagen.jpg:   codec_name=mjpeg   width=640 height=360 bit_rate=2748400
imagen.webp:  codec_name=webp    width=640 height=360
audio.wav:    codec_name=pcm_s16le sample_rate=44100 channels=1 bit_rate=705717
audio.mp3:    codec_name=mp3       sample_rate=44100 channels=1 bit_rate=193769
audio.ogg:    codec_name=vorbis    sample_rate=44100 channels=1 bit_rate=24112
video.mp4:    codec_name=h264      width=640 height=360  + aac 44100 1ch  bit_rate=138840
video.webm:   codec_name=vp9       width=640 height=360  + opus 48000 1ch bit_rate=428856
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`ffprobe` es el primer comando que se lanza ante un fichero que "no se reproduce": dice el códec real, no el que promete la extensión.</span>

## 7. Convertir un fichero: `ffmpeg`

Los cuatro comandos que se usaron, tal cual:

```bash
# imagen: PNG a JPEG con calidad 2 (menor número, más calidad)
ffmpeg -i imagen.png -q:v 2 imagen.jpg

# imagen: PNG a WebP con calidad 80
ffmpeg -i imagen.png -quality 80 imagen.webp

# audio: WAV a MP3 a 192 kbps
ffmpeg -i audio.wav -b:a 192k audio.mp3

# audio: WAV a OGG (Vorbis) con calidad 5
ffmpeg -i audio.wav -q:a 5 audio.ogg

# vídeo: MP4 a WebM con VP9
ffmpeg -i video.mp4 -c:v libvpx-vp9 -b:v 500k -c:a libopus video.webm
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`-q:v` controla la calidad en imagen y vídeo; `-b:a` fija el bitrate de audio. Sin esos parámetros, `ffmpeg` usa sus valores por defecto y el resultado puede sorprender.</span>

## 8. Lo que se puede usar en Android

Android no reproduce cualquier cosa: tiene una **lista oficial de formatos soportados** por versión. Antes de dar por bueno un MP4, se consulta esa lista.

| Tipo | Formatos seguros en Android |
|---|---|
| Imagen | JPEG, PNG, WebP |
| Audio | MP3, AAC, OGG/Vorbis, WAV (PCM) |
| Vídeo | H.264 en MP4, H.265, VP9 en WebM |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La lista de formatos soportados de Android es la referencia: si el fichero no está ahí, la app puede funcionar en el emulador y fallar en un móvil real.</span>

## 9. Lo que tienes que llevarte de este apartado

- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Contenedor es la caja; códec es la compresión. La extensión no basta para saber qué hay dentro.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">PNG para zonas planas, JPEG para fotos, WebP para las dos cosas con transparencia.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">WAV guarda sin comprimir; MP3, OGG y AAC comprimen con pérdida.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El bitrate decide el tamaño: 192 kbps en audio es calidad alta.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`ffprobe` mira y `ffmpeg` convierte. Se mide el resultado antes de afirmar que mejora.</span>

## Bibliografía y documentación oficial

- Android — formatos multimedia soportados: [developer.android.com/media/platform/supported-formats](https://developer.android.com/media/platform/supported-formats).
- Android — APIs de multimedia: [developer.android.com/media](https://developer.android.com/media).
- FFmpeg — documentación oficial: [ffmpeg.org/documentation.html](https://ffmpeg.org/documentation.html).
- FFmpeg — `ffprobe`: [ffmpeg.org/ffprobe.html](https://ffmpeg.org/ffprobe.html).
- MDN — guía de formatos de imagen: [developer.mozilla.org — Image file types](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Image_types).
- MDN — contenedores multimedia: [developer.mozilla.org — Media containers](https://developer.mozilla.org/en-US/docs/Web/Media/Guides/Formats/Containers).
- Google — WebP: [developers.google.com/speed/webp](https://developers.google.com/speed/webp).
- Módulo 0489 — Real Decreto 405/2023 (BOE): [BOE-A-2023-13221](https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221).

## Presentación

- [PM-U5 - Contenidos multimedia](https://revilofe.github.io/slides/section1-pmdm/PM-U5.-ContenidosMultimedia.html)
