---
title: "Práctica UD5 - Mi app multimedia"
summary: "Práctica evaluable de la unidad 5: capturar, procesar, reproducir y animar contenido multimedia en una app Android."
series: "Programación de Dispositivos Móviles"
series_order: 5
section: "5. Contenidos multimedia"
author: Ismael Velasco
date: 2026-10-25
weight: 5
---
# Práctica UD5 - Mi app multimedia

**Módulo:** Programación Multimedia y Dispositivos Móviles (0489) · **RA3** · **CE** a, b, c, d, e, f, g, h · **RD 405/2023**
**Sesiones:** 2 · **Entrega:** repositorio + capturas + `gradlew test`

## Qué hay que construir

Una app Android con **una sola pantalla** y cuatro acciones:

1. **Capturar** una foto con la cámara y guardarla en el almacenamiento de la app.
2. **Procesar** esa foto: pasarla a escala de grises y generar una copia a la mitad de tamaño.
3. **Reproducir** un vídeo corto y un audio, mostrando en pantalla el estado y la posición.
4. **Animar** una secuencia de al menos 4 fotogramas y un elemento que cambie de tamaño.

## Requisitos (checklist)

- [ ] Permiso `CAMERA` declarado en el manifiesto **y** pedido en ejecución.
- [ ] Foto guardada en `filesDir` con nombre único.
- [ ] Escala de grises calculada con los coeficientes 0,299 / 0,587 / 0,114.
- [ ] Copia reducida a la mitad con `createScaledBitmap`.
- [ ] Medidas en pantalla: bytes y dimensiones de la foto, del gris y de la reducida.
- [ ] Reproductor Media3 con `play`, `pause` y `seekTo(0)`.
- [ ] Estado del reproductor visible (`idle`, `cargando`, `listo`, `terminado`).
- [ ] Error de reproducción controlado con `onPlayerError` y mostrado en pantalla.
- [ ] Secuencia de 4 fotogramas en bucle.
- [ ] Animación por propiedades disparada por un botón.
- [ ] `release()` del reproductor al salir de la pantalla.
- [ ] README con las decisiones y las capturas.

## Orden de trabajo (minutos)

| Tramo | Qué se hace | Min |
|---|---|---|
| 1 | Proyecto, dependencias (CameraX 1.6.2, Media3 1.11.1) y permisos | 20 |
| 2 | Captura y guardado de la foto | 25 |
| 3 | Procesado (gris + reducida) y medidas en pantalla | 25 |
| 4 | Reproductor con estados y error provocado | 25 |
| 5 | Animaciones (fotogramas + propiedades) | 15 |
| 6 | README, capturas y `gradlew test` | 10 |

## Entrega

- Repositorio con el proyecto completo.
- **6 capturas**: permiso pedido, foto capturada, medidas en pantalla, vídeo reproduciendo, error provocado, animación en dos fotogramas distintos.
- Salida de `gradlew test` pegada en el README.

## Rúbrica (sobre 10)

| Apartado | Peso |
|---|---|
| Captura con CameraX y guardado correcto | 2 |
| Procesado del Bitmap (gris con coeficientes y reducción) | 2 |
| Medidas reales en pantalla | 1 |
| Reproducción con estados y controles | 2 |
| Error controlado y mostrado | 1 |
| Animaciones (fotogramas y propiedades) | 1,5 |
| Código comentado, README y capturas | 0,5 |

## Descuentos

- Captura sin el código visible o sin medidas: **−1**.
- `release()` ausente: **−1**.
- Escala de grises con media simple en vez de coeficientes: **−0,5**.
- Animación con `Thread.sleep` en el hilo principal: **−1**.

## Bibliografía oficial

- CameraX: [developer.android.com/media/camera/camerax](https://developer.android.com/media/camera/camerax).
- Media3 ExoPlayer: [developer.android.com/media/media3/exoplayer](https://developer.android.com/media/media3/exoplayer).
- Animaciones en Compose: [developer.android.com/develop/ui/compose/animation](https://developer.android.com/develop/ui/compose/animation).
- `Bitmap`: [developer.android.com/reference/android/graphics/Bitmap](https://developer.android.com/reference/android/graphics/Bitmap).
- Módulo 0489 — RD 405/2023: [BOE-A-2023-13221](https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221).
