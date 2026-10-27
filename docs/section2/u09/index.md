---
title: "UD 9 - Documentación y ayudas"
description: "Índice de la unidad 9: la documentación de la aplicación — ayuda contextual, estructura de los datos que se guardan, manual de usuario, guía de instalación y tutoriales, generados desde el propio código (RA6 completo)."
authors:
    - Ismael Velasco
date: 2026-10-27
icon: "material/book-open-page-variant"
permalink: /di/unidad9/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - documentación
    - ayuda contextual
    - manual de usuario
    - Markdown
---

# UD 9 — Documentación y ayudas

Una aplicación sin documentación no está terminada: está a medias. Esta unidad construye la
documentación del `IsmaelUIKit` **desde el propio código** —la ayuda que cambia según la pantalla, la
tabla de los datos que se guardan y un **manual que la app se escribe a sí misma** con su versión y sus
permisos reales— y lo prueba con seis pruebas automáticas. Sin copiar y pegar textos que se quedan
viejos: si cambia la app, cambia el manual.

## Contenido

- [Teoría 9.1 — Documentación y ayudas](teoria/DI-U9.1.-DocumentacionYAyudas.md)

## Qué aprenderás

- Los **cinco tipos de ayuda** —en línea, contextual, tutorial, manual y referencia— y para qué sirve
  cada uno.
- Los **sistemas de generación** que se usan hoy: docs-as-code con Markdown y MkDocs o Docusaurus,
  Dokka y Javadoc para la referencia, wikis y centros de ayuda.
- Los **formatos** habituales, incluidos los que ya no se usan (CHM y HLP, que se preguntan en examen).
- Hacer **ayuda sensible al contexto**: <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">la misma clave que usa la navegación sirve para pedir la ayuda</span>.
- Documentar la **estructura de la información persistente**: dónde está cada dato, de qué tipo es y
  **cuándo se borra**.
- Generar el **manual de usuario** y la **guía de instalación** leyendo el paquete instalado, no de
  memoria.
- Escribir un **tutorial** que se pueda seguir, y saber qué hace la IA y qué no.

## Proyecto de la unidad

El mismo `IsmaelUIKit`, ampliado con la escena 10:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | Todo lo anterior + `Ayuda` (ayuda por pantalla), `EsquemaDatos` (los datos y su borrado) y `Manual` (el manual generado) |
| `:app` | `com.example.uikit.demo` | Diez escenas: la última es **Documentación y ayuda**, con la ayuda contextual, la tabla del esquema y el botón de generar el manual |
| pruebas | `com.example.uikit` (androidTest) | Las 29 anteriores + `DocumentacionTest` con 6 pruebas nuevas — **35 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto: las tres capturas son del emulador, el
`manual.md` publicado es el fichero que escribió la app (2.909 bytes) y la salida de pruebas
(`OK (35 tests)` · `Time: 47.621`) es la que devolvió `am instrument`.

## Resultado de aprendizaje

**RA6. Documenta aplicaciones seleccionando y utilizando herramientas específicas** — la unidad lo
trabaja **completo**: los siete criterios de evaluación, de la `a` a la `g`.

| CE | Qué se hace en la unidad |
| -- | ------------------------ |
| a | Los cinco tipos de ayuda y las herramientas que los generan |
| b | La tabla de formatos: Markdown, HTML, PDF, EPUB, dentro de la app, CHM y JavaHelp |
| c | `Ayuda.para(clave)`: la ayuda cambia según la pantalla, con ayuda general de respaldo |
| d | `EsquemaDatos`: almacén, tipo, descripción y borrado de cada dato, en tabla y en JSON |
| e | `Manual.generar()`: el manual de usuario y la referencia, escritos desde la app instalada |
| f | La ficha de instalación, configuración y administración con datos reales |
| g | El tutorial de la escena 10, con pasos que se pueden comprobar |

## Relación con otros módulos

La documentación se apoya en todo lo anterior: la **navegación** de la UD2 da las claves de las
pantallas, los **componentes** de la UD4 y el **estado** de la UD5 son lo que la app guarda, y las
**pruebas** de la UD5 y la UD6 son las que se amplían aquí. Lo que en esta unidad se documenta es
también lo que la **UD11** convierte en pruebas del sistema completo.
