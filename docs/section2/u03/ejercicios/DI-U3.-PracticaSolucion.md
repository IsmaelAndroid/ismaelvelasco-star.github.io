---
title: "UD 3 - Solucionario de la práctica: XML vivo en tu app Compose"
description: "Soluciones completas y comentadas de los tres ejercicios de la práctica UD 3: externalización, logo vectorial y parseo XmlPullParser con LazyColumn."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/check-decagram"
permalink: /di/unidad3/practica-soluciones
categories:
    - DI
tags:
    - DI
    - XML
    - Kotlin
    - Jetpack Compose
---

# Solucionario UD 3 — Práctica: XML vivo en tu app Compose

> **Uso docente.** Este documento contiene las soluciones de la práctica. No se publica en la web de estudiantes.

## Solución del Ejercicio 1 — Externalización

**`res/values/strings.xml`:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_titulo">XML Vivo</string>
    <string name="pantalla_recursos">Recursos</string>
    <string name="pantalla_datos">Datos</string>
    <string name="logo_desc">Logotipo de FourWaves: cuatro ondas superpuestas</string>
    <string name="maravillas_cargadas">%1$d maravillas cargadas desde XML</string>
    <string name="hello_blank">Hello Android!</string>
</resources>
```

Observa el patrón de **parámetro posicional** `%1$d` en `maravillas_cargadas`: así el texto se traduce con el número en el sitio correcto de cada idioma.

**`res/values-en/strings.xml`** (creada con Locale = en):

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_titulo">XML Alive</string>
    <string name="pantalla_recursos">Resources</string>
    <string name="pantalla_datos">Data</string>
    <string name="logo_desc">FourWaves logo: four overlapping waves</string>
    <string name="maravillas_cargadas">%1$d wonders loaded from XML</string>
</resources>
```

No hace falta re-declarar `hello_blank` en `values-en`: si falta una clave en un idioma, Android cae al `values/` por defecto.

**En el componible:**

```kotlin
@Composable
fun Cabecera() {
    Column(horizontalAlignment = Alignment.CenterHorizontally) {
        Text(stringResource(R.string.app_titulo), style = MaterialTheme.typography.headlineMedium)
        Text(stringResource(R.string.pantalla_recursos), style = MaterialTheme.typography.labelLarge)
    }
}
```

**Errores típicos comentados:**

- *Hardcoded string* persistente: suele quedar el literal en el `android:label` del manifest — cámbialo por `@string/app_titulo`.
- Apóstrofe sin escapar (`<string name="x">It's alive</string>`) rompe el build: `It\'s alive`.
- Mezclar `values/` y `values-en/` con claves distintas (no traducciones): la app cambia de idioma y "pierde" textos. Las claves deben ser las mismas en todos los idiomas.

## Solución del Ejercicio 2 — Logo vectorial

Tras **New → Vector Asset → Local file**, el `res/drawable/ic_fourwaves.xml` queda así (pathData generado a partir de las elipses — arcos elípticos `a`):

```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="125dp"
    android:height="37.5dp"
    android:viewportWidth="500"
    android:viewportHeight="150">
    <path
        android:fillColor="#7CB342"
        android:pathData="M20,100 a220,30 0 1,0 440,0 a220,30 0 1,0 -440,0"/>
    <path
        android:fillColor="#4DD0E1"
        android:pathData="M30,70 a190,20 0 1,0 380,0 a190,20 0 1,0 -380,0"/>
    <path
        android:fillColor="#F48FB1"
        android:pathData="M40,45 a170,15 0 1,0 340,0 a170,15 0 1,0 -340,0"/>
    <path
        android:fillColor="#FFF176"
        android:pathData="M65,25 a140,12 0 1,0 280,0 a140,12 0 1,0 -280,0"/>
</vector>
```

Puntos clave del XML resultante:

- `viewportWidth/Height` = el lienzo del SVG original (500×150). El drawable escala a cualquier tamaño manteniendo proporciones.
- Cada `<ellipse>` se convierte en un `<path>` con dos arcos elípticos (por eso el `a` aparece dos veces: la elipse son dos semicírculos unidos).
- `width/height` en `dp` es el tamaño "nominal" para previsualizar; en Compose lo dominas tú con el `Modifier`.

**En el componible:**

```kotlin
Image(
    painter = painterResource(R.drawable.ic_fourwaves),
    contentDescription = stringResource(R.string.logo_desc),
    modifier = Modifier
        .fillMaxWidth()
        .height(64.dp)
        .padding(horizontal = 16.dp)
)
```

El `contentDescription` sale del `strings.xml` del ejercicio 1: accesibilidad y externalización suman.

**Sobre el reto del error AAPT:** al escribir `android:fillColour`, el build falla con `error: attribute 'android:fillColour' not found` — el compilador de recursos valida el vocabulario (XML válido ≠ XML bien formado: lo viste en teoría §3.4).

## Solución del Ejercicio 3 — Parseo de maravillas

**`res/xml/maravillas.xml`** (idéntico al de la teoría §5.2; se omite aquí por brevedad — raíz `<maravillas>`, siete `<maravilla>` con `<nombre>` y `<pais>`).

**`Maravilla.kt`:**

```kotlin
data class Maravilla(val nombre: String, val pais: String)
```

**`Parseo.kt` — la función completa (los TODO resueltos):**

```kotlin
import android.content.Context
import org.xmlpull.v1.XmlPullParser

fun parsearMaravillas(context: Context): List<Maravilla> {
    val lista = mutableListOf<Maravilla>()
    var nombre = ""
    var pais = ""
    val parser = context.resources.getXml(R.xml.maravillas)

    while (parser.eventType != XmlPullParser.END_DOCUMENT) {
        when (parser.eventType) {
            XmlPullParser.START_TAG -> when (parser.name) {
                "nombre" -> nombre = parser.nextText()
                "pais" -> pais = parser.nextText()
            }
            XmlPullParser.END_TAG -> if (parser.name == "maravilla") {
                lista += Maravilla(nombre, pais)
                nombre = ""; pais = ""          // reinicio para la siguiente
            }
        }
        parser.next()
    }
    return lista
}
```

Comentarios de profesor:

- `nextText()` avanza el parser hasta el texto y **devuelve el contenido** del elemento — por eso no hay que capturar el evento `TEXT` a mano.
- El reinicio `nombre = ""; pais = ""` evita arrastrar valores si algún `<maravilla>` viniera incompleto (XML de datos sucio). Con `requireNotNull` o valores vacíos detectables podrías además registrar el error.
- Es un parser **pull**: tú pides el siguiente evento (`parser.next()`), a diferencia de SAX que te empuja callbacks. Con documentos pequeños es indiferente; con XML grandes, pull te deja abortar cuando quieras.

**`MainActivity.kt` — pantalla completa:**

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            MaterialTheme {
                XMLVivoApp()
            }
        }
    }
}

@Composable
fun XMLVivoApp() {
    val contexto = LocalContext.current
    val maravillas = remember { parsearMaravillas(contexto) }

    Column(Modifier.fillMaxSize()) {
        Cabecera()                       // ejercicio 1
        Image(                           // ejercicio 2
            painter = painterResource(R.drawable.ic_fourwaves),
            contentDescription = stringResource(R.string.logo_desc),
            modifier = Modifier.fillMaxWidth().height(64.dp)
        )
        Text(
            stringResource(R.string.maravillas_cargadas, maravillas.size),
            style = MaterialTheme.typography.labelLarge,
            modifier = Modifier.padding(16.dp)
        )
        LazyColumn(
            verticalArrangement = Arrangement.spacedBy(8.dp),
            contentPadding = PaddingValues(16.dp)
        ) {
            items(maravillas) { m ->
                Card(modifier = Modifier.fillMaxWidth()) {
                    Column(Modifier.padding(12.dp)) {
                        Text(m.nombre, style = MaterialTheme.typography.titleMedium)
                        Text(m.pais, style = MaterialTheme.typography.bodySmall,
                             color = MaterialTheme.colorScheme.onSurfaceVariant)
                    }
                }
            }
        }
    }
}
```

Detalles que se evalúan:

- `remember { parsearMaravillas(...) }`: el parseo ocurre **una vez** por composición de la pantalla; al rotar, Compose reutiliza el resultado guardado (y si no, lo recalcula en la recreación — nunca dos parseos en la misma vida de la pantalla). La rotación es el test gratis de que no hiciste el trabajo en cada ` recomposición`.
- `stringResource(R.string.maravillas_cargadas, maravillas.size)` rellena el `%1$d` — fíjate en cómo el recurso viaja con su parámetro.
- `items(maravillas)` con data class: cada fila se identifica por igualdad estructural; funciona, aunque en listas enormes convendría `key = { it.nombre }`.

**Extensión para nota (no exigida):** convertir el parser a función de extensión y testearlo unitariamente extrayendo el `XmlPullParser` de un `String` con `Xml.newPullParser()` + `setInput(StringReader(xml))` — así el test no necesita `Context` ni recursos Android.
