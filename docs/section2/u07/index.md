---
title: "UD 7 - Interfaces naturales"
description: "Índice de la unidad 7: las interfaces que no se manejan con botones — voz, movimiento del cuerpo, detección de partes del cuerpo con ML Kit y realidad aumentada con Scene Viewer (RA2 completo)."
authors:
    - Ismael Velasco
date: 2026-10-27
icon: "material/microphone"
permalink: /di/unidad7/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - interfaces naturales
    - voz
    - sensores
    - ML Kit
    - realidad aumentada
---

# UD 7 — Interfaces naturales

Hablar, mover el móvil, mirar, apuntar con la cámara. Esta unidad construye las **cuatro entradas
naturales** del RA2 dentro del mismo `IsmaelUIKit` de las unidades anteriores, y las cuatro con código
de verdad, no con maquetas: un intérprete de comandos de voz que entiende «abre el piano» aunque el
reconocedor devuelva tildes y mayúsculas, un detector de sacudidas con su freno por tiempo, la
detección de caras de **ML Kit** dibujando la caja y los **diez puntos** sobre la imagen, y la
realidad aumentada abierta con el **visor de escenas** del sistema a partir de un `.glb`.

## Contenido

- [Teoría 7.1 — Interfaces naturales](teoria/DI-U7.1.-InterfacesNaturales.md)

## Qué aprenderás

- Qué es una **interfaz natural** (NUI) y por qué <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">no sustituye a los botones: los acompaña</span>.
- Identificar las **herramientas de aprendizaje automático** que se usan hoy en Android: ML Kit (caras,
  pose, texto, códigos), MediaPipe, TensorFlow Lite/LiteRT, ARCore, `SpeechRecognizer`, `TextToSpeech`
  y `SensorManager`, y **qué devuelve cada una**.
- **Reconocer voz** con `RecognizerIntent` (sin pedir permisos) y con `SpeechRecognizer` (con
  `RECORD_AUDIO`), y **normalizar** el texto antes de interpretarlo.
- Convertir una frase en una **acción** con un vocabulario cerrado (`IrA`, `Siguiente`, `Anterior`,
  `Ayuda`, `Desconocida`) y <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">no inventar nunca una acción cuando no se entiende</span>.
- Leer el **acelerómetro**: magnitud `√(x²+y²+z²)`, gravedad, umbral y freno entre sacudidas, con el
  sensor registrado y dado de baja en `DisposableEffect`.
- Detectar **partes del cuerpo** con ML Kit Face Detection: caja, los **diez puntos** y sus códigos
  reales (`NOSE_BASE = 6`, `RIGHT_EYE = 10`…), que **no van seguidos**.
- Integrar **realidad aumentada** con el visor del sistema: anatomía de la *intent URI*, el bloque
  `#Intent;…`, el permiso `<queries>` de Android 11 y la comprobación de ARCore antes de lanzar.
- Escribir **pruebas automáticas** de todo lo anterior sin micrófono ni cámara: la interpretación de
  voz, el umbral de la sacudida, los nombres de los puntos y la conversión de la URI en intent.

## Proyecto de la unidad

El mismo `IsmaelUIKit`, ampliado con la escena 8:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | Todo lo de las UD4, UD5 y UD6 + `ComandoVoz`, `DetectorSacudida`, `Rostro` y `RealidadAumentada` |
| `:app` | `com.example.uikit.demo` | Ocho escenas: catálogo, figuras, recarga de abono, puntero, piano, persistencia, auditoría y **la escena natural** |
| `:ui` (dependencias) | — | **ML Kit Face Detection 16.1.7** (modelo incluido en la app, funciona sin red) |
| pruebas | `com.example.uikit` (androidTest) | Las 16 de las unidades anteriores + `InterfazNaturalTest` con 7 pruebas nuevas — **23 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto, compilado y ejecutado en el emulador:
las tres capturas son del emulador, los resultados de voz, sacudida y rostro están pegados tal cual
salieron y la salida de pruebas (`OK (23 tests)` · `Time: 35.654`) es la que devolvió `am instrument`.

## Resultado de aprendizaje

**RA2. Genera interfaces naturales de usuario utilizando herramientas visuales** — la unidad lo trabaja
**completo**: los seis criterios de evaluación, de la `a` a la `f`.

| CE | Qué se hace en la unidad |
| -- | ------------------------ |
| a | Tabla de herramientas de aprendizaje automático y de lo que devuelve cada una |
| b | La escena 8 completa, con las cuatro entradas en la misma pantalla |
| c | `ComandoVoz` + `RecognizerIntent` + `TextToSpeech` |
| d | Acelerómetro, `DetectorSacudida` y contador con freno |
| e | ML Kit Face Detection, caja y diez puntos dibujados |
| f | Scene Viewer por *intent URI* y comprobación de ARCore |

## Relación con otros módulos

Las cuatro entradas naturales se apoyan en lo construido antes: los **componentes propios** de la UD4
y la **persistencia del estado** de la UD5 son los que permiten que la escena natural tenga tarjetas
reutilizables y que el contador de sacudidas sobreviva a un giro de pantalla. Y lo que aquí se prueba
con tests es lo que en la **UD8** se documenta en el informe del RA5: capturas, tabla de pruebas y
conclusiones.
