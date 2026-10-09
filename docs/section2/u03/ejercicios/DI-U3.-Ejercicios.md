---
title: "UD 3 - Ejercicios: Generación de interfaces a partir de documentos XML"
description: "Relación de ejercicios del tema 3, de menor a mayor dificultad: Bloque A (conceptos XML), Bloque B (recursos Android en XML) y Bloque C (parseo y mini-proyectos)."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/puzzle"
permalink: /di/unidad3/ejercicios
categories:
    - DI
tags:
    - DI
    - XML
    - Kotlin
    - Jetpack Compose
---

# Ejercicios UD 3 — Generación de interfaces a partir de documentos XML

Trabaja en tu proyecto `EjerciciosUD3` (Empty Activity). Los del Bloque A se responden sobre el papel o comentando en clase; los B y C son de código.

## Bloque A — Conceptos (calentamiento)

**A1.** Señala si estos fragmentos XML están **bien formados** y, si no, di por qué (hay al menos tres errores distintos entre los que fallan):

```xml
<!-- (i) -->
<cliente nombre="Ana">
  <pedido id="7"/>
</cliente>

<!-- (ii) -->
<Cliente><nombre>Ana</Nombre></Cliente>

<!-- (iii) -->
<cliente nombre="Ana" nombre="García"/>

<!-- (iv) -->
<maravilla>
  Chichén Itzá
  <pais>México</pais>
</maravilla>
```

**A2.** Explica la diferencia entre un documento XML **bien formado** y uno **válido**, y pon el ejemplo de cada caso con vocabulario Android (`<vector>`, `android:fillColor`).

**A3.** Del SVG al componible: ordena los pasos del flujo y di qué herramienta interviene en cada uno. (Pasos desordenados: `painterResource` · Vector Asset · clase `R` · AAPT · SVG del diseñador.)

**A4.** Une con flechas cada evento clásico con su forma en Compose: Click · Change · GetFocus · gesto de mantener pulsado. (Opciones: `onValueChange` · `Modifier.pointerInput` + `detectTapGestures` · `onFocusChanged` vía `Modifier` · `onClick`.)

## Bloque B — Recursos Android en XML (práctica guiada)

**B1. Corrige el drawable.** Este vector drawable no compila. Encuentra los **dos** errores y ponlo bien:

```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24dp" android:height="24dp"
    android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColour="#FF0000"
        android:pathData="M12,4 L20,20 L4,20"/>
</vector>
```

**B2. El string escurridizo.** `strings.xml` rechaza esta línea. Explícalo y arréglalo **sin cambiar el texto**:

```xml
<string name="lema">No hay 'b' en "XML"</string>
```

**B3. Tres idiomas.** Añade a tu proyecto `values-en/` y `values-fr/` para las cadenas `app_name`, `saludo` y `despedida`. ¿Qué ocurre si el móvil está en alemán? Razona la respuesta con la regla de *fallback* de recursos.

**B4. Icono propio.** Dibuja (a mano o con Figma/Inkscape) un icono simple de 24×24 —una casa, una nube, lo que quieras—, expórtalo a SVG e impórtalo con Vector Asset. Muéstralo en un `Icon` dentro de un `FloatingActionButton`.

## Bloque C — Parseo y mini-proyectos (integración)

**C1. Podcasts desde RSS.** Escribe un documento `res/xml/podcast.xml` con un `<channel>` que contenga tres `<item>` (con `<title>` y `<duration>`). Párselo con `XmlPullParser` (patón maravillas, pero con dos niveles: saltas `<channel>` sin recogerla) y pinta título + duración en una `LazyColumn`.

**C2. El menú del comedor.** Modelo `res/xml/menu.xml` con `<semana>` → `<dia nombre="...">` → `<plato tipo="primero|segundo|postre">`. Parsea a una `Map<String, List<Plato>>` y muestra con pestañas o desplegables por día. Pista: los atributos se leen con `parser.getAttributeValue(null, "nombre")`.

**C3. Validador de XML bien formado (para nota).** Un `TextField` donde se pegue XML, un botón "Validar" y un texto que diga si está bien formado. Pista: `Xml.newPullParser().apply { setInput(StringReader(entrada)) }` dentro de un `try/catch` — la excepción te da la línea del error. Es la versión Kotlin del error de AAPT que viste en la teoría.
