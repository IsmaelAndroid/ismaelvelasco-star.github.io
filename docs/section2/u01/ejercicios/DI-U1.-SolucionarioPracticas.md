---
title: "UD 1 - Solucionario de prácticas (P3, P4 y P5)"
description: Soluciones comentadas de las prácticas de contenedores, filas e imágenes de la unidad 1, con el enunciado de cada ejercicio.
summary: "Solucionario de las prácticas P3 (Column y botones), P4 (Row y Card) y P5 (imágenes y recursos): cada solución precedida de su enunciado."
authors:
    - Ismael Velasco
date: 2026-10-06
icon: "material/file-document-edit"
permalink: /di/unidad1/solucionario-practicas
categories:
    - DI
tags:
    - DI
    - Solucionario
    - Compose
    - Row
    - Column
    - Button
    - Card
    - Image

toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

# Solucionario de las prácticas P3, P4 y P5

Cada solución va precedida de su **enunciado resumido** (el completo, con diagramas, está en cada práctica). Todo el código está **verificado compilando** (`compileDebugKotlin EXIT:0`).

!!! warning
    Este documento existe para **autocomprobarte DESPUÉS de pelear** con los diagramas de cada práctica. Si tu solución difiere pero funciona y está razonada, también vale.

## Práctica 3 — Column, botones y contador

### P3 · Ejercicio 1 — La Column maestra

> **Enunciado.** Una Column que ocupe toda la pantalla con 16dp de margen, hijos separados 12dp y centrados horizontalmente, con un título de 22sp.

```kotlin
Column(
    modifier = Modifier
        .fillMaxSize()      // toda la pantalla...
        .padding(16.dp),    // ...menos 16dp por dentro
    verticalArrangement = Arrangement.spacedBy(12.dp),
    horizontalAlignment = Alignment.CenterHorizontally
) {
    Text("Contador de pulsaciones", fontSize = 22.sp)
}
```

**Comentario.** El `modifier` se lee de izquierda a derecha: primero "ocupa todo", luego "respeta 16dp por dentro". Si inviertes el orden el resultado visible cambia (el padding se aplicaría antes de medir). Los tres atributos de una Column siempre son estos tres: `modifier`, eje principal (`verticalArrangement`) y eje perpendicular (`horizontalAlignment`).

### P3 · Ejercicio 2 — Botones con personalidad

> **Enunciado.** SUMAR: verde con contentPadding ancho. REINICIAR: rojo y desactivado si el contador está a cero.

```kotlin
Button(
    onClick = { pulsaciones = pulsaciones + 1 },
    colors = ButtonDefaults.buttonColors(containerColor = Color(0xFF2E7D32)),
    contentPadding = PaddingValues(start = 24.dp, end = 24.dp, top = 12.dp, bottom = 12.dp)
) { Text("SUMAR") }

Button(
    onClick = { pulsaciones = 0 },
    enabled = pulsaciones > 0,
    colors = ButtonDefaults.buttonColors(containerColor = Color(0xFFB3261E))
) { Text("REINICIAR") }
```

**Comentario.** `enabled` es el atributo "estado" del botón: con `false` se ve gris y no responde. Como lo conectamos a `pulsaciones > 0`, Compose lo re-evalúa en cada recomposición — nadie tiene que "activarlo a mano". Fíjate en que el color es ARGB: `FF` = opaco, `2E7D32` = verde.

### P3 · Ejercicio 3 — El contador visible

> **Enunciado.** El estado y el número grande que se actualiza solo.

```kotlin
var pulsaciones by remember { mutableIntStateOf(0) }
// ...
Text("$pulsaciones", fontSize = 48.sp)
```

**Comentario.** Receta de tres pasos: declarar con `remember` → modificar dentro de un `onClick` → leerlo en un `Text`. La interpolación `"$pulsaciones"` incrusta el valor actual. Cuando el estado cambia, Compose vuelve a ejecutar SOLO los composables que lo leen: por eso el número cambia al instante y el resto de la pantalla no se toca.

### P3 · Ejercicio 4 — Comprobación visual (respuestas)

1. Sin `horizontalAlignment`: **compila** (no es obligatorio) y el contenido se alinea a `Start` — todo a la izquierda. Familia silenciosa.
2. Con `Arrangement.Center`: el GRUPO de hijos se centra verticalmente como bloque (el hueco total se reparte arriba y abajo del grupo, no entre hijos). `spacedBy` solo mete separación fija sin mover el grupo.
3. REINICIAR está gris porque `enabled = pulsaciones > 0` y al arrancar `pulsaciones == 0`. La primera pulsación a SUMAR recomposición y lo activa.

## Práctica 4 — Row y Card

### P4 · Ejercicio 1 — Botones en fila

> **Enunciado.** Los dos botones en una Row con SpaceEvenly.

```kotlin
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.SpaceEvenly,
    verticalAlignment = Alignment.CenterVertically
) {
    Button(onClick = { pulsaciones = pulsaciones + 1 }, /* ... */) { Text("SUMAR") }
    Button(onClick = { pulsaciones = 0 }, enabled = pulsaciones > 0 /* ... */) { Text("REINICIAR") }
}
```

**Comentario.** Ojo al espejo: en el Row el `Arrangement` es ahora el eje X y el `Alignment` el Y — justo al revés que en la Column del contenedor padre. `fillMaxWidth()` significa "todo el ancho QUE ME DEJA la Column padre" (que ya tiene su padding de 16dp).

### P4 · Ejercicio 2 — El laboratorio de Arrangement

> **Enunciado.** izq/centro/der con cada modo y captura.

```kotlin
Text("— SpaceBetween —", fontSize = 12.sp)
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.SpaceBetween,  // cambia por SpaceAround / SpaceEvenly / Center
    verticalAlignment = Alignment.CenterVertically
) { Text("izq"); Text("centro"); Text("der") }
```

**Comentario.** Cómo reconocerlos:

- **SpaceBetween**: izq PEGADO al borde izquierdo, der al derecho; todo el hueco en los dos huecos internos.
- **SpaceAround**: cada hijo con su "burbuja" de hueco. Los extremos tienen media burbuja contra el borde; los del medio, dos medias (una por lado).
- **SpaceEvenly**: los CUATRO huecos (incluidos los de los extremos) miden EXACTO lo mismo.
- **Center**: grupo junto en el centro; dos huecos grandes (izquierda y derecha del grupo).

### P4 · Ejercicio 3 — La Card del marcador

> **Enunciado.** El número de 48sp dentro de una Card azul con sombra.

```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    colors = CardDefaults.cardColors(containerColor = Color(0xFF1F4E79)),
    elevation = CardDefaults.cardElevation(defaultElevation = 6.dp)
) {
    Text(
        "$pulsaciones",
        fontSize = 48.sp,
        color = Color.White,
        modifier = Modifier.fillMaxWidth(),
        textAlign = TextAlign.Center
    )
}
```

**Comentario.** La Card es el contenedor "etiqueta" de Material: fondo propio, esquinas redondeadas y `elevation` (sombra que la separa visualmente del fondo). Como todo contenedor de Compose, usa el patrón slot: dentro va lo que tú quieras. Para centrar el texto dentro de una Card ancha: `fillMaxWidth()` + `textAlign = TextAlign.Center` (el `horizontalAlignment` de una Column no aplica aquí porque el Text es hijo directo).

### P4 · Ejercicio 4 — Mini-toolbar (anidar)

> **Enunciado.** Una Row con dos Column dentro.

```kotlin
Row(
    modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.SpaceBetween,
    verticalAlignment = Alignment.CenterVertically
) {
    Column(horizontalAlignment = Alignment.CenterHorizontally) {
        Text("Puntos", fontSize = 12.sp)
        Text("$pulsaciones", fontSize = 16.sp)
    }
    Column(horizontalAlignment = Alignment.CenterHorizontally) {
        Text("Sesión", fontSize = 12.sp)
        Text("UD1", fontSize = 16.sp)
    }
}
```

**Comentario.** Anidar es gratis: cada contenedor aplica sus reglas a SUS hijos directos. La Row reparte las dos Column en los extremos; cada Column centra a sus dos Text. Así se construye cualquier pantalla: cajas dentro de cajas.

## Práctica 5 — Imágenes y recursos

### P5 · Ejercicio 1 — Meter el recurso

> **Enunciado.** Un PNG en res/drawable con nombre válido.

Carpeta: `app/src/main/res/drawable/mi_imagen.png`. El truco de verificación: escribe `R.drawable.` en el editor — si el autocompletado ofrece tu imagen, el recurso está bien registrado. Si `R` sale en rojo tras copiar el archivo: **Build → Clean Project** y compila (la clase R se regenera en cada build).

**Comentario.** Las reglas del nombre (minúsculas/números/guión bajo) existen porque el nombre se convierte en un identificador de la clase `R` — un nombre de variable Java/Kotlin válido. `DemoBotones.png` rompería el build con un error de recurso inválido.

### P5 · Ejercicios 2 y 3 — Pintar y mostrar/ocultar

> **Enunciado.** La imagen con Image, el if que la oculta y los tres botones (incluido el toggle).

```kotlin
var imagenVisible by remember { mutableStateOf(true) }
// ...
if (imagenVisible) {
    Image(
        painter = painterResource(R.drawable.mi_imagen),
        contentDescription = "Descripción de la imagen",
        modifier = Modifier.size(240.dp)
    )
} else {
    Text("(imagen oculta)", fontSize = 16.sp,
         modifier = Modifier.size(240.dp), textAlign = TextAlign.Center)
}

Row(modifier = Modifier.fillMaxWidth(),
    horizontalArrangement = Arrangement.SpaceEvenly,
    verticalAlignment = Alignment.CenterVertically) {
    Button(onClick = { imagenVisible = true })  { Text("Mostrar") }
    Button(onClick = { imagenVisible = false }) { Text("Ocultar") }
    Button(onClick = { imagenVisible = !imagenVisible }) {
        Text(if (imagenVisible) "Ocultar" else "Mostrar")
    }
}
```

**Comentario.** Aquí el estado no cambia CÓMO se ve un composable: decide SI EXISTE. Al cambiar `imagenVisible`, Compose ejecuta la otra rama del `if` (a eso lo llamamos composición condicional). El toggle (`!imagenVisible`) es el patrón clásico de un solo botón con dos funciones, y su etiqueta cambia sola porque el `Text` también se re-evalúa.

### P5 · Ejercicio 4 — Selector de tres fotos

> **Enunciado.** Tres imágenes y un selector con when.

```kotlin
var fotoActual by remember { mutableIntStateOf(1) }
// ...
when (fotoActual) {
    1 -> Image(painterResource(R.drawable.foto1), "Foto 1", Modifier.size(240.dp))
    2 -> Image(painterResource(R.drawable.foto2), "Foto 2", Modifier.size(240.dp))
    else -> Image(painterResource(R.drawable.foto3), "Foto 3", Modifier.size(240.dp))
}
// botones: onClick = { fotoActual = 1 } etc.
```

**Comentario.** Un `when` sobre un estado es la forma idiomática de "pestañas" en Compose sin bibliotecas. En el repositorio de soluciones la galería demo usa la misma imagen en las tres ramas (para que compile sin que tengas que crear recursos); en tu entrega pon TRES imágenes distintas.

## Errores típicos de la trilogía (por familia)

| Si te sale | Causa | Familia |
|---|---|---|
| `Unresolved reference 'Card'` / `'CardDefaults'` | falta el import de material3 | B — Import |
| `Unresolved reference 'painterResource'` | falta `androidx.compose.ui.res.painterResource` | B — Import |
| `Unresolved reference 'R'` o `'drawable'` | PNG mal nombrado, o recién copiado → Clean + compilar | B — Import (recurso) |
| `No value passed for parameter 'onClick'` | parámetro obligatorio olvidado | C — Parámetro |
| El contador no se actualiza | el `by remember` quedó FUERA del composable, o el Text no lee la variable | Silenciosa |
| Todo pegado en la esquina | falta el Arrangement/Alignment (o el padding) — compila pero se ve mal | Silenciosa |
| La imagen no se ve pero no hay error | el nombre del archivo ≠ `R.drawable.loquesea` | Silenciosa |

## Código completo de referencia

Las soluciones completas (archivos `.kt` listos para copiar en tu proyecto, con el recurso `demo_botones.png`) están en el repositorio de soluciones: **[ismaelvelasco-star/di-soluciones](https://github.com/ismaelvelasco-star/di-soluciones)**.
