---
title: "Castillo de Luna 4: Componentes reutilizables en Compose"
description: La práctica de componentes a medida de JavaFX con exportación a .jar, traducida a composables personalizados reutilizables entre proyectos.
summary: "Botón que cambia de tamaño, botón que cambia su texto y un componente creativo propio; cada uno en su fichero, reutilizado en otro proyecto."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/widgets"
permalink: /di/castillo/p4
categories:
    - DI
tags:
    - DI
    - Componentes
    - Animaciones
    - Reutilización
---

# Práctica Castillo 4: Componentes reutilizables

La original pedía componentes a medida para **JavaFX** exportados en un `.jar` para usarlos
desde otro proyecto. En Compose un componente a medida es simplemente **un `@Composable` con
sus parámetros** viviendo en su propio fichero, y "exportarlo" es tenerlo en una librería o
módulo reutilizable. La competencia es la misma: **construir piezas propias y consumirlas
donde haga falta**.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Botón que cambia de tamaño

Crea un componente a medida: un botón que **al pulsarse cambia de tamaño** (de pequeño a
grande, y al revés). Debe vivir en su propio fichero (`BotonCambiaTamano.kt`) con **al menos
un parámetro configurable** (el texto). Úsalo en una pantalla de otro proyecto (o de otra
actividad del mismo proyecto, en un módulo distinto).

### 2. Botón que cambia su texto

Crea un componente a medida: un botón con el texto `Púlsame` que **al tocarlo** cambie su
texto a `Púlsame!!`, y al soltar/repulsar vuelva a `Púlsame`. Fichero propio
(`BotonCambiaTexto.kt`) y reutilizado en otra pantalla.

### 3. Componente personal

Crea un componente a medida **a tu gusto**, con alguna idea que se te ocurra (una tarjeta con
imagen y estado de "favorito", un contador animado, un interruptor con icono...). **Se premia
la creatividad.** Fichero propio, al menos dos parámetros configurables, y reutilizado en una
pantalla.

### Extra (sube nota)

Documenta cómo harías para compartir tus tres componentes con un compañero **sin copiar el
fichero**: módulo de librería Android (o Kotlin Multiplatform) referenciado desde Gradle.
Dos párrafas y un esquema valen.

## Consejos

- Toca y mira la preview: `animateDpAsState` y `animateColorAsState` hacen el trabajo sucio
  de las animaciones por ti.
- Parámetros con valores por defecto = componente usable sin leer la documentación.
- Capturas del componente en acción en la pantalla que lo consume.

## Importante

Entrega en **PDF** con capturas válidas. Retraso = 1.
