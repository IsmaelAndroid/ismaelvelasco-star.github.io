---
title: "UD 5 - Explotación de componentes visuales"
description: "Índice de la unidad 5: eventos, persistencia del estado, introspección por semántica y pruebas unitarias e instrumentadas sobre los componentes propios, en Jetpack Compose (RA3 y RA1 g)."
authors:
    - Ismael Velasco
date: 2026-10-26
icon: "material/cursor-default-click-outline"
permalink: /di/unidad5/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - eventos
    - pruebas
---

# UD 5 — Explotación de componentes visuales

En la UD4 fabricaste componentes. Aquí se **explotan**: que respondan a los eventos que les corresponden,
que su estado sobreviva a girar el móvil, que se puedan inspeccionar desde fuera y que haya pruebas
automáticas que avisen antes de que lo descubra el usuario. Los dos casos prácticos —la
configuración del ratón y el teclado de piano— llegan resueltos con `pointerInput`, `rememberSaveable` y
Compose Testing.

## Contenido

- [Teoría 5.1 — Explotación de componentes visuales](teoria/DI-U5.1.-ExplotacionDeComponentes.md)

## Qué aprenderás

- Desmontar un evento en sus piezas (fuente, objeto, oyente, acción) y **traducir** cada listener del
  Java/Swing a su equivalente de 2026: `MouseListener` → `pointerInput`, `KeyListener` → `onKeyEvent`,
  `FocusListener` → `onFocusEvent`, `ChangeListener` → `onValueChange`.
- Decidir **a qué eventos responde** cada componente y a cuáles no (un componente de dibujo no responde
  a ninguno: solo recibe), y associar a cada uno una acción.
- **Introspección**: el árbol de semántica, `testTagsAsResourceId`, y leer el volcado real de
  `uiautomator` para ver nombre, caja de toque y estado accesible de cada pieza.
- **Persistencia**: la diferencia exacta entre `remember` y `rememberSaveable`, y cómo se escribe un
  `Saver` para un tipo de dato propio.
- **Pruebas**: qué va a `src/test` y qué a `src/androidTest`, `createComposeRule`, `onNodeWithTag`,
  `performTouchInput`, `assertThrows`, y cómo se lanzan desde el terminal.

## Proyecto de la unidad

El mismo `IsmaelUIKit` de la UD4, ampliado:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | `Estado`, `BadgeEstado`, `SelectorMes`, `LienzoFiguras` (UD4) + `ConfigPuntero` y su `Saver`, `PanelPuntero`, `FiguraRaton`, `Nota`, `Tecla`, `TecladoPiano` |
| `:app` | `com.example.uikit.demo` | Seis escenas: catálogo, figuras, recarga de abono, **configuración del puntero**, **teclado de piano** y **persistencia al girar**, más `NotaReproductor` (`AudioTrack`) |
| pruebas | `com.example.uikit` (androidTest) | `BadgeEstadoTest`, `PanelPunteroTest`, `TeclaEventosTest`, `ConfigPunteroTest` — **11 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto, compilado con Android Gradle Plugin 9.4.1,
Kotlin 2.2.10 y Compose BOM 2026.02.01, y ejecutado en el emulador: las cuatro capturas son del emulador y
la salida de pruebas y los volcados de `uiautomator` están pegados tal cual salieron.

## Resultado de aprendizaje

**RA3. Crea componentes visuales valorando y empleando herramientas específicas** — cierra los criterios
**d)** (eventos y acciones) y **e)** (pruebas unitarias), repasa el **c)** (propiedades con defecto) y el
**h)** (app que usa los componentes), y trabaja el **RA1 g)** (asociar acciones a eventos).

## Relación con otros módulos

El evento que cambia el estado y ese estado que se pinta es **Desarrollo de Interfaces**. En cuanto ese
estado hay que guardarlo en un disco, pedirlo por red o compartirlo entre pantallas con un `ViewModel`, se
entra en **PMDM** (módulo 0489): por eso en esta unidad la persistencia se limita a la **persistencia de la
interfaz** (`Bundle`, cambios de configuración) y el sonido del piano se genera en el momento, sin base de
datos ni archivos de por medio.
