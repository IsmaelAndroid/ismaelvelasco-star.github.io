---
title: "Solucionario Castillo 4"
description: Tres componentes reutilizables en Compose: botón animado de tamaño, botón con texto reactivo y tarjeta favorito creativa.
summary: "Solución completa de la práctica Castillo 4."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s4
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 4

## Ejercicio 1 — BotónCambiaTamano.kt

```kotlin
@Composable
fun BotonCambiaTamano(texto: String, onClick: () -> Unit = {}) {
    var grande by remember { mutableStateOf(false) }             // estado del tamaño

    val alto by animateDpAsState(                                 // dp animado
        targetValue = if (grande) 96.dp else 48.dp,
        animationSpec = spring(dampingRatio = Spring.DampingRatioMediumBouncy),
        label = "alto"
    )
    val ancho by animateDpAsState(if (grande) 240.dp else 140.dp, label = "ancho")

    Button(
        onClick = { grande = !grande; onClick() },   // toggle + callback al consumidor
        modifier = Modifier.height(alto).width(ancho)
    ) { Text(texto) }
}
```

Consumido en otra pantalla (aquí está la gracia de los parámetros):

```kotlin
BotonCambiaTamano(texto = "¡Hazme grande!")      // sin más: valores por defecto
BotonCambiaTamano(texto = "Modo fiesta", onClick = { musica = !musica })
```

## Ejercicio 2 — BotonCambiaTexto.kt

```kotlin
@Composable
fun BotonCambiaTexto(
    normal: String = "Púlsame",
    pulsado: String = "Púlsame!!"
) {
    var presionado by remember { mutableStateOf(false) }

    // interactionSource reporta el ciclo presión/solta del botón
    val inter = remember { MutableInteractionSource() }
    LaunchedEffect(inter) {
        inter.interactions.collect { ev ->
            when (ev) {
                is PressInteraction.Press   -> presionado = true    // dedo arriba
                is PressInteraction.Release,
                is PressInteraction.Cancel  -> presionado = false   // dedo fuera
            }
        }
    }

    Button(onClick = {}, interactionSource = inter) {
        Text(if (presionado) pulsado else normal)
    }
}
```

> Variante de andar por casa si `MutableInteractionSource` asusta: cambiar el texto en
> `onClick` y restaurar con un `LaunchedEffect` + `delay(800)`. Evaluable, menos fina.

## Ejercicio 3 — TarjetaFavorito.kt (propuesta creativa)

```kotlin
@Composable
fun TarjetaFavorito(
    titulo: String,
    subtitulo: String,
    inicialFavorito: Boolean = false,
    alCambiar: (Boolean) -> Unit = {}
) {
    var fav by remember { mutableStateOf(inicialFavorito) }

    val colorFondo by animateColorAsState(
        if (fav) MaterialTheme.colorScheme.tertiaryContainer
                else MaterialTheme.colorScheme.surfaceVariant,
        label = "fondo"
    )

    Card(colors = CardDefaults.cardColors(containerColor = colorFondo)) {
        Row(Modifier.padding(16.dp), verticalAlignment = Alignment.CenterVertically) {
            Column(Modifier.weight(1f)) {
                Text(titulo, style = MaterialTheme.typography.titleMedium)
                Text(subtitulo, style = MaterialTheme.typography.bodyMedium)
            }
            IconButton(onClick = { fav = !fav; alCambiar(fav) }) {
                Icon(
                    imageVector = if (fav) Icons.Filled.Favorite else Icons.Filled.FavoriteBorder,
                    contentDescription = if (fav) "Quitar de favoritos" else "Añadir a favoritos",
                    tint = if (fav) MaterialTheme.colorScheme.error
                           else MaterialTheme.colorScheme.onSurface
                )
            }
        }
    }
}
```

## Extra — compartir sin copiar el fichero

1. Crear un **módulo de librería** (`:ui-componentes`) en el proyecto: *File → New → New
   Module → Android Library*. Los tres ficheros de componentes viven ahí.
2. La app que los consume declara `implementation(project(":ui-componentes"))` en su
   `build.gradle.kts` y hace `import` normal.
3. Para compartirlo ENTRE proyectos: publicar el módulo en un repositorio Maven (local con
   `mavenLocal()` o privado) y consumirlo por coordenadas `implementation("es.iesra:ui:1.0")`.
   Es el equivalente exacto del `.jar` de la práctica original: misma idea, mismo propósito,
   packaging moderno.
