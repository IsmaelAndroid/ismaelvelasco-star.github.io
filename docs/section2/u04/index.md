---
title: "UD 4 - Creación de componentes propios"
description: "Índice de la unidad 4: crear componentes visuales en Jetpack Compose con propiedades, eventos, DrawScope, pruebas, documentación y empaquetado en un módulo de librería (RA3)."
authors:
    - Ismael Velasco
date: 2026-10-26
icon: "material/view-dashboard"
permalink: /di/unidad4/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - componentes
---

# UD 4 — Creación de componentes propios

La unidad en la que dejas de **usar** componentes y empiezas a **fabricarlos**: una píldora de estado, un
desplegable de meses y un lienzo dibujado a mano, escritos por ti, con sus propiedades, sus eventos, sus
pruebas, su documentación y su paquete reutilizable.

## Contenido

- [Teoría 4.1 — Creación de componentes visuales](teoria/DI-U4.1.-CreacionDeComponentes.md)

## Presentación

- [DI-U4.1 - Creación de componentes visuales](https://revilofe.github.io/slides/section6-di/DI-U4.1.-CreacionDeComponentes.html)

## Qué aprenderás

- Convertir un trocito de interfaz que se repite en un **componente reutilizable** con parámetros y
  valores por defecto bien elegidos.
- Qué es una **propiedad** y cómo se traduce el patrón JavaBean a parámetros de un
  `@Composable`.
- A pintar a mano con **`DrawScope`** (el `Graphics` de toda la vida: óvalo, rectángulo, `Path`, trazos
  y degradados) y a meter imágenes vectoriales, empaquetadas y remotas.
- **Diseñar y probar sin móvil**: `@Preview`, variantes, `@PreviewParameter` y Layout Inspector.
- **Probar de verdad** el componente con `createComposeRule` y `testTag`, y leer la salida de las pruebas.
- **Empaquetar** el kit en un módulo de librería (`.aar`) y consumirlo desde la app o desde otro proyecto.

## Proyecto de la unidad

`IsmaelUIKit`, dos módulos:

| Módulo | Paquete | Qué contiene |
| ------ | ------- | ------------ |
| `:ui` | `com.example.uikit` | `Estado`, `BadgeEstado`, `SelectorMes`, `LienzoFiguras` y sus pruebas |
| `:app` | `com.example.uikit.demo` | La app que usa el kit en tres escenas (catálogo, figuras, recarga de abono) |

Todo el código de la teoría es el código real de ese proyecto, compilado con Android Gradle Plugin 9.4.1,
Kotlin 2.2.10 y Compose BOM 2026.02.01, y ejecutado en el emulador.

## Resultado de aprendizaje

**RA3. Crea componentes visuales valorando y empleando herramientas específicas** — criterios de
evaluación a) a h), los ocho completos. Detalles y correspondencia con cada apartado en la propia teoría.

## Relación con otros módulos

Este es el contenido propio de **Desarrollo de Interfaces**: *cómo se construye la pieza*. La integración
de esa pieza con datos, red y persistencia se ve en **PMDM** (módulo 0489), así que aquí el kit **no toca
Room, Retrofit ni ViewModel**: `BadgeEstado` recibe un estado y lo pinta, punto.
