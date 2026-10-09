---
title: "UD 3 - Generación de interfaces a partir de documentos XML"
description: "Índice de la unidad 3: teoría, práctica, ejercicios y solucionario de XML y su papel en el Android moderno con Kotlin y Jetpack Compose."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/xml"
permalink: /di/unidad3/
categories:
    - DI
tags:
    - DI
    - XML
    - Jetpack Compose
---

# UD 3 — Generación de interfaces a partir de documentos XML

La unidad que explica por qué XML sigue vivo en un mundo Compose: recursos, drawables vectoriales, documentos de datos y eventos — y cómo conectar todo con Kotlin.

## Contenido

- [Teoría 3.1 — Generación de interfaces a partir de documentos XML](teoria/DI-U3.1.-GeneracionInterfacesXML.md)
- [Práctica UD 3: XML vivo en tu app Compose](practica/DI-U3.-Practica.md)
- [Ejercicios UD 3](ejercicios/DI-U3.-Ejercicios.md)
- [Solucionario UD 3](ejercicios/DI-U3.-Solucionario.md)

## Qué aprenderás

- Leer, validar y editar cualquier documento XML de un proyecto Android (manifest, strings, drawables, datos).
- El flujo moderno del gráfico vectorial: SVG → Vector Asset → clase R → `painterResource` en Compose.
- Los lenguajes derivados de XML que importan hoy (SVG, RSS, XSLT como idea de transformación) y los que fueron historia (editores dedicados, WYSIWYG).
- Eventos clásicos frente a Compose: de `android:onClick` en XML a `onClick = { }` verificado por el compilador.
- Parsear documentos XML de datos con `XmlPullParser` y pintarlos en una `LazyColumn`.
