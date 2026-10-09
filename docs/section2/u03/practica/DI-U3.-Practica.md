---
title: "UD 3 - Práctica: XML vivo en tu app Compose"
description: "Práctica integrada de la unidad 3: recursos XML (strings, drawables vectoriales), un documento XML de datos embebido y su parseo con XmlPullParser, consumido todo desde Jetpack Compose."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/xml"
permalink: /di/unidad3/practica
categories:
    - DI
tags:
    - DI
    - XML
    - Kotlin
    - Jetpack Compose
---

# Práctica UD 3 — XML vivo en tu app Compose

Una práctica, tres ejercicios encadenados: primero externalizas textos a `strings.xml`, luego importas un SVG como drawable vectorial, y por último parseas un documento XML de datos con `XmlPullParser` y lo pintas en una lista Compose. Todo en un único proyecto `XMLVivo`.

**Duración estimada:** 2 sesiones (2 h 30 min).

**Entrega:** proyecto comprimido o repositorio Git con el commit final y capturas del emulador con las tres pantallas.

## Preparación

1. Crea un proyecto nuevo: **Empty Activity**, nombre `XMLVivo`, paquete `com.ismael.xmlvivo`, mínimo SDK 24.
2. Comprueba que en `res/values/` ya tienes `strings.xml` y `themes.xml` — los vas a usar.

## Ejercicio 1 — Externalización: nada de textos hardcoded (30 min)

La plantilla trae `Text("Hello Android!")` escrito a fuego en el componible. Eso impide traducir la app y dispersa los textos por el código.

1. Abre `res/values/strings.xml` y añade tres cadenas: `app_titulo`, `pantalla_recursos` y `pantalla_datos` (valores a tu gusto, p. ej. *XML Vivo*, *Recursos*, *Datos*).
2. En `MainActivity.kt` (o el componible que toque), sustituye los literales por `stringResource(R.string.app_titulo)` y compañía.
3. Añade un `strings.xml` de carpeta `values-en/` (New → Values Resource File → Locale → en) con las tres cadenas en inglés. Cambia el idioma del emulador (Settings → System → Languages) y verifica que la app cambia sola.
4. **Comprobación:** lint debe quedar limpio de warnings *Hardcoded string*.

> **Nota:** si algún texto contiene un apóstrofe o comillas, escápalos en el XML (`\'`) — es la primera vez que te peleas con las reglas de escape de atributos y contenido XML.

## Ejercicio 2 — El logo vectorial: del SVG al drawable (30 min)

La emisora FourWaves (caso práctico de la teoría) quiere su logo en la app.

1. Descarga o crea este SVG (guárdalo como `fourwaves.svg` en tu ordenador):

    ```xml
    <svg xmlns="http://www.w3.org/2000/svg" height="150" width="500">
      <ellipse cx="240" cy="100" rx="220" ry="30" fill="#7CB342"/>
      <ellipse cx="220" cy="70"  rx="190" ry="20" fill="#4DD0E1"/>
      <ellipse cx="210" cy="45"  rx="170" ry="15" fill="#F48FB1"/>
      <ellipse cx="205" cy="25"  rx="140" ry="12" fill="#FFF176"/>
    </svg>
    ```

2. En Android Studio: clic derecho en `res/drawable/` → **New → Vector Asset** → Local file (SVG) → selecciona `fourwaves.svg` → nómbralo `ic_fourwaves`.
3. Abre el `ic_fourwaves.xml` generado y localiza el `<vector>` con su `viewportWidth/Height` y los `<path>` con `fillColor`. Compara mentalmente: cada `<ellipse>` del SVG es un arco en `pathData`.
4. En la pantalla principal, debajo del título, muestra el logo con `Image(painterResource(R.drawable.ic_fourwaves), contentDescription = stringResource(R.string.logo_desc))` y `Modifier.fillMaxWidth().height(64.dp)`.
5. **Reto (optativo):** fuerza un error a propósito — cambia `android:fillColor` por `android:fillColour` y lee el error de AAPT. Vuelve a dejarlo bien.

## Ejercicio 3 — Datos en XML: maravillas parseadas (1 h)

El plato fuerte: un documento XML **de datos** (el de las 7 maravillas de la teoría), metido en `res/xml/` y parseado con `XmlPullParser` para pintarlo en una `LazyColumn`.

1. Crea la carpeta `res/xml/` (New → Android Resource Directory → Resource type: XML) y dentro `maravillas.xml` con las siete maravillas (nombre y país — el documento exacto está en la teoría §5.2).
2. Crea la data class:

    ```kotlin
    data class Maravilla(val nombre: String, val pais: String)
    ```

3. Escribe esta función y **complétala tú** donde van los `TODO`:

    ```kotlin
    fun parsearMaravillas(context: Context): List<Maravilla> {
        val lista = mutableListOf<Maravilla>()
        var nombre = ""
        var pais = ""
        val parser = context.resources.getXml(R.xml.maravillas)
        while (parser.eventType != XmlPullParser.END_DOCUMENT) {
            when (parser.eventType) {
                XmlPullParser.START_TAG -> when (parser.name) {
                    // TODO 1: si la etiqueta es "nombre" o "pais",
                    //         lee el texto del elemento (parser.nextText())
                }
                XmlPullParser.END_TAG -> if (parser.name == "maravilla") {
                    // TODO 2: añade a lista un Maravilla(nombre, pais)
                }
            }
            parser.next()
        }
        return lista
    }
    ```

4. En el componible, recupera la lista con `remember { parsearMaravillas(context) }` y píntala en una `LazyColumn`: cada fila es una `Card` con el nombre en `titleMedium` y el país en `bodySmall` gris.
5. **Pantalla de datos con contador:** añade arriba un `Text` que diga *"7 maravillas cargadas desde XML"* usando el `size` de la lista.
6. **Comprobación final:** rota el emulador y verifica que la lista no se re-parsea dos veces (el `remember` la protege).

## Rúbrica (orientativa)

| Criterio | Peso |
|---|---|
| Ej. 1: externalización completa + values-en funcional | 25 % |
| Ej. 2: SVG importado, mostrado con painterResource | 25 % |
| Ej. 3: parseo correcto de las 7 maravillas | 35 % |
| Estilo y limpieza (Card, espaciados, sin warnings lint) | 15 % |

## ¿Qué has practicado?

- Escribir y editar documentos XML reales de un proyecto Android (strings, drawable vectorial, datos).
- El flujo SVG → Vector Asset → `R.drawable` → composable.
- Parsear XML con `XmlPullParser`: eventos `START_TAG`/`END_TAG`, `nextText()`, acumulación de estado.
- Consumir recursos XML desde Compose (`stringResource`, `painterResource`).
