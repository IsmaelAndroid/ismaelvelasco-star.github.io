---
title: "UD 3 - Solucionario: Ejercicios de XML"
description: "Soluciones comentadas de los ejercicios del tema 3: conceptos XML, recursos Android, parseo con XmlPullParser y el validador bien formado."
authors:
    - Ismael Velasco
date: 2026-10-09
icon: "material/check-decagram"
permalink: /di/unidad3/ejercicios-soluciones
categories:
    - DI
tags:
    - DI
    - XML
    - Kotlin
    - Jetpack Compose
---

# Solucionario UD 3 — Ejercicios

> **Uso docente.** Contiene las soluciones de los ejercicios. No se publica en la web de estudiantes.

## Bloque A — Conceptos

**A1.**

- **(i) Bien formado.** Atributo único y citado, etiqueta vacía cerrada con `/>`, anidamiento correcto.
- **(ii) MAL: anidamiento cruzado y case-sensitivity.** `<Cliente>` abre con mayúscula y `</Nombre>` cierra `nombre` — en XML las etiquetas son *case-sensitive* y `nombre ≠ Nombre`; además el cierre exterior no coincide con `<Cliente>`. Correcto: `<cliente><nombre>Ana</nombre></cliente>`.
- **(iii) MAL: atributo repetido.** Dos `nombre` en el mismo elemento — los atributos de un elemento son únicos (regla §3.3). Correcto: `nombre="Ana" apellido="García"` o elementos hijos.
- **(iv) MAL: mezcla texto + elementos.** El elemento `maravilla` contiene a la vez texto suelto (`Chichén Itzá`) y un hijo (`<pais>`). Un elemento contiene o texto o elementos, no ambos. Correcto: `<maravilla><nombre>Chichén Itzá</nombre><pais>México</pais></maravilla>`.

**A2.** *Bien formado* = cumple las reglas sintácticas de XML (etiquetas cerradas, anidadas, case-sensitive, atributos únicos citados). *Válido* = además obedece a un vocabulario/schema externo. Ejemplo Android: un `<vector>` con `<path android:fillColor="...">` correctamente escrito pero con atributo inventado `android:pintar="rojo"` está **bien formado** pero **no válido** — AAPT lo rechaza porque el schema Android no conoce `android:pintar`.

**A3.** Flujo ordenado: **1)** SVG del diseñador → **2)** Vector Asset (importador de Android Studio) genera el XML drawable → **3)** AAPT compila los recursos y genera la **clase R** con el identificador → **4)** `painterResource(R.drawable.ic_x)` lo consume en el componible.

**A4.** Click → `onClick` · Change → `onValueChange` · GetFocus → `onFocusChanged` (vía `Modifier`) · mantener pulsado → `Modifier.pointerInput` + `detectTapGestures` (con `onLongPress`).

## Bloque B — Recursos Android

**B1.** Los dos errores: (1) `android:fillColour` — el atributo correcto es **`android:fillColor`** (case-sensitive, error típico de AAPT: *"attribute not found"*); (2) falta la etiqueta de cierre `>` ... en realidad el `>` del `<vector>` está, el fallo real es que `M12,4 L20,20 L4,20` **no cierra el subpath**: un pathData debe terminar con `Z` (close) o simplemente los comandos son válidos — el drawable compila igual. **La solución canónica:** el único error que impide compilar es `fillColour`. Corregido (y con `Z` para cerrar el triángulo, que es lo intencionado):

```xml
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="24dp" android:height="24dp"
    android:viewportWidth="24" android:viewportHeight="24">
    <path android:fillColor="#FF0000"
        android:pathData="M12,4 L20,20 L4,20 Z"/>
</vector>
```

*(Nota docente: si algún grupo señala además el `Z` como "tercer" arreglo, es válido discutirlo — el path sin `Z` pinta el triángulo igual porque el fill cierra automáticamente.)*

**B2.** El problema son las **comillas dobles dentro de un valor delimitado por comillas dobles** (y los apóstrofes, que Android exige escapar en recursos de cadena). Solución con escape:

```xml
<string name="lema">No hay \'b\' en \"XML\"</string>
```

Alternativa equivalente envolviendo el valor en comillas simples dobles... la forma canónica Android es la de arriba: `\'` para apóstrofe y `\"` para comilla.

**B3.** `values-en/strings.xml`:

```xml
<resources>
    <string name="app_name">My App</string>
    <string name="saludo">Hello</string>
    <string name="despedida">Goodbye</string>
</resources>
```

`values-fr/strings.xml`:

```xml
<resources>
    <string name="app_name">Mon App</string>
    <string name="saludo">Bonjour</string>
    <string name="despedida">Au revoir</string>
</resources>
```

Con el móvil en **alemán**, Android busca `values-de/`, no lo encuentra, y aplica el **fallback**: cae a la carpeta por defecto `values/` (español). Regla: se usa la mejor coincidencia de configuración y, si no hay, el valor por defecto — por eso `values/` debe ser completo y no depender de que exista `values-en/`.

**B4.** Solución tipo con una nube (SVG de ejemplo):

```xml
<!-- nube.svg -->
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24">
  <path d="M6,18 h12 a4,4 0 0 0 0,-8 a6,6 0 0 0 -11,-2 a4.5,4.5 0 0 0 -1,10 z"
        fill="#90CAF9"/>
</svg>
```

Importado como `ic_nube` y mostrado:

```kotlin
FloatingActionButton(onClick = { /* ... */ }) {
    Icon(painterResource(R.drawable.ic_nube), contentDescription = "Nueva nube")
}
```

## Bloque C — Parseo y mini-proyectos

**C1. Podcasts.** `res/xml/podcast.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<channel>
  <item><title>Interfaces declarativas</title><duration>32</duration></item>
  <item><title>El retorno de XML</title><duration>27</duration></item>
  <item><title>Compose y los recursos</title><duration>41</duration></item>
</channel>
```

Parser y pantalla:

```kotlin
data class Episodio(val titulo: String, val duracion: Int)

fun parsearPodcast(context: Context): List<Episodio> {
    val eps = mutableListOf<Episodio>()
    var titulo = ""
    var dur = 0
    val p = context.resources.getXml(R.xml.podcast)
    while (p.eventType != XmlPullParser.END_DOCUMENT) {
        when (p.eventType) {
            XmlPullParser.START_TAG -> when (p.name) {
                "title" -> titulo = p.nextText()
                "duration" -> dur = p.nextText().toInt()
            }
            XmlPullParser.END_TAG -> if (p.name == "item") {
                eps += Episodio(titulo, dur)
                titulo = ""; dur = 0
            }
        }
        p.next()
    }
    return eps
}

@Composable
fun ListaPodcast() {
    val contexto = LocalContext.current
    val episodios = remember { parsearPodcast(contexto) }
    LazyColumn(verticalArrangement = Arrangement.spacedBy(8.dp),
               contentPadding = PaddingValues(16.dp)) {
        items(episodios) { e ->
            ListItem(headlineContent = { Text(e.titulo) },
                     supportingContent = { Text("${e.duracion} min") })
        }
    }
}
```

Observa que `<channel>` no se recoge: el `when` solo actúa en las etiquetas que interesan — el resto de eventos pasan por el flujo sin hacer nada. Eso es lo que permite "saltarse" niveles.

**C2. Menú del comedor.** `res/xml/menu.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<semana>
  <dia nombre="Lunes">
    <plato tipo="primero">Lentejas</plato>
    <plato tipo="segundo">Merluza</plato>
    <plato tipo="postre">Flan</plato>
  </dia>
  <dia nombre="Martes">
    <plato tipo="primero">Gazpacho</plato>
    <plato tipo="segundo">Pollo asado</plato>
    <plato tipo="postre">Yogur</plato>
  </dia>
</semana>
```

Parser a `Map<String, List<Plato>>`:

```kotlin
data class Plato(val tipo: String, val nombre: String)

fun parsearMenu(context: Context): Map<String, List<Plato>> {
    val menu = linkedMapOf<String, MutableList<Plato>>()
    var diaActual = ""
    val p = context.resources.getXml(R.xml.menu)
    while (p.eventType != XmlPullParser.END_DOCUMENT) {
        when (p.eventType) {
            XmlPullParser.START_TAG -> when (p.name) {
                "dia" -> diaActual = p.getAttributeValue(null, "nombre")!!
                "plato" -> menu.getOrPut(diaActual) { mutableListOf() } +=
                    Plato(p.getAttributeValue(null, "tipo")!!, p.nextText())
            }
        }
        p.next()
    }
    return menu
}
```

La clave nueva es **`getAttributeValue(null, "nombre")`**: el `null` indica "sin namespace". El `getOrPut` crea la lista del día la primera vez que aparece. Pantalla con desplegables (`ExposedDropdownMenuBox`, visto en UD 2) o simplemente pestañas con `TabRow` — se deja al gusto del grupo.

**C3. Validador bien formado (para nota).**

```kotlin
@Composable
fun ValidadorXML() {
    var entrada by remember { mutableStateOf("") }
    var resultado by remember { mutableStateOf<String?>(null) }

    Column(Modifier.padding(16.dp), verticalArrangement = Arrangement.spacedBy(8.dp)) {
        OutlinedTextField(
            value = entrada,
            onValueChange = { entrada = it; resultado = null },
            label = { Text("Pega tu XML") },
            modifier = Modifier.fillMaxWidth().height(180.dp)
        )
        Button(onClick = {
            resultado = try {
                Xml.newPullParser().apply {
                    setInput(StringReader(entrada))
                }.let {
                    while (it.eventType != XmlPullParser.END_DOCUMENT) it.next()
                    "✔ Bien formado"
                }
            } catch (e: Exception) {
                "✘ ${e.message}"     // incluye línea y columna del error
            }
        }) { Text("Validar") }
        resultado?.let {
            Text(it, color = if (it.startsWith("✔")) Color(0xFF2E7D32) else Color(0xFFC62828))
        }
    }
}
```

La gracia pedagógica: la excepción de `XmlPullParser` reporta **posición (línea, columna)** del primer fallo de formación — es exactamente la información que te da AAPT cuando rompe el build por un drawable mal escrito, ahora en tiempo real y en manos de tu alumnado.
