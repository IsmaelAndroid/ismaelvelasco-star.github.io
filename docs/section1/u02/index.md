---
title: "UD 2 - Programación en Android: IU con Jetpack Compose"
description: "Índice de la unidad 2 de PMDM: fundamentos de Kotlin, actividades e intents, y la app Compose con listas y formularios."
authors:
    - Ismael Velasco
date: 2026-10-25
icon: "material/file-document-outline"
permalink: /pmdm/unidad2/
categories: PMDM
tags:
    - PMDM
    - Kotlin
    - Android
    - Activities
    - Intents
    - Jetpack Compose

# Relacionado con la tabla de contenidos
toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

## **UD 2 - Programación en Android: IU con Jetpack Compose**

Esta es la unidad donde tu código deja de ser una pantalla suelta y pasa a ser **una app**: con ciclo de vida, con varias pantallas que se pasan datos, que llama por teléfono, comparte fotos y contesta a URLs propias. Todo el código de la teoría está ejecutado en un emulador real (las capturas son evidencia, no aspiraciones).

### 1. Normativa que Respalda la Unidad

Unidad alineada con el módulo **Programación Multimedia y Dispositivos Móviles** (0489) del ciclo DAM, cuyo currículo vigente fija el **RD 405/2023** (que actualiza el RD 450/2010), en torno al **Resultado de Aprendizaje 2** —desarrolla aplicaciones para dispositivos móviles analizando y empleando las tecnologías y librerías específicas— y sus criterios de evaluación **a)** estructura de clases de la aplicación, **b)** clases que modelan ventanas, menús, alertas y controles, **g)** pruebas de interacción usuario-aplicación en emulador, **h)** empaquetado y despliegue en dispositivo real, **i)** documentación de los procesos y **j)** permisos requeridos. El texto íntegro de RA y CE está en [`recursos/PMDM Normativa.txt`](../recursos/PMDM%20Normativa.txt).

### Teoría

- [2.1.- Fundamentos del lenguaje Kotlin](teoria/PMDM-U2.1.-FundamentosLenguajeKotlin.md) — 14 bloques de código ejecutado: tipos, null-safety, funciones, lambdas, colecciones y POO con comparación Java frente a frente.
- [2.2.- Actividades e Intents: la anatomía de una app Android](teoria/PMDM-U2.2.-ActividadesEIntents.md) — ciclo de vida, Bundle de rotación, intents explícitos/implícitos, filtros propios y PendingIntents.
- [2.3.- La app Compose: Scaffold, listas Lazy y formulario](teoria/PMDM-U2.3.-AppComposeListasYFormulario.md) — la interfaz con mirada de app: andamio Material, listas que escalan, validación de formulario.

### Prácticas

- Práctica 2.1 (mini-proyecto): «Tu libreta, la nuestra» — 2 sesiones: monta tu propia versión de FumetoContactos con tus datos y tus decisiones. Enunciado: [PMDM-U2.-MiniProyectoContactos](practica/PMDM-U2.-MiniProyectoContactos.md)

> Los enunciados de las prácticas se entregan en el aula a través de la plataforma del curso.

### Presentaciones

* [PM-U2.1 - Fundamentos del lenguaje Kotlin](https://revilofe.github.io/slides/section1-pmdm/PM-U2.1.-FundamentosKotlin.html)
* [PM-U2.2 - Actividades e Intents](https://revilofe.github.io/slides/section1-pmdm/PM-U2.2.-ActividadesIntents.html)
* [PM-U2.3 - La app Compose](https://revilofe.github.io/slides/section1-pmdm/PM-U2.3.-AppComposeListasYFormulario.html)

### Situación en el módulo

| UD | Contenido | Estado |
|---|---|---|
| UD 1 | Análisis de tecnologías móviles | ✅ publicada |
| **UD 2** | **IU con Compose (esta)** | **en curso** |
| UD 3 | Gestión de datos (preferencias, Room, red) | en preparación |
| UD 4 | Multimedia, sensores y despliegue | en preparación |
| UD 5-6 | Motores de juegos y realidad aumentada | 2.º trimestre |
