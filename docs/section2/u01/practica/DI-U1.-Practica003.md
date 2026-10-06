---
title: "UD 1 - P3: Contenedores y atributos (Row, Column, Button, Card)"
description: "Práctica de atributos: todos los parámetros de Row y Column, atributos de Button, código dentro del onClick y un contenedor tipo etiqueta (Card). Con solución comentada y diagramas del resultado esperado en el emulador."
summary: "Toca cada atributo de Row/Column, personaliza botones, escribe código real en onClick y monta una Card — con diagramas de cómo debe quedar la app."
authors:
    - Ismael Velasco
date: 2026-10-06
icon: "material/file-document-edit"
permalink: /di/unidad1/p3
categories:
    - DI
tags:
    - DI
    - Compose
    - Kotlin
    - Row
    - Column
    - Button
    - Card

toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

# Práctica 3: Contenedores y sus atributos

En las prácticas anteriores aprendiste a **leer** el código y a **romperlo** a propósito. Ahora toca **construir**: vas a montar una pantalla donde cada atributo de `Row` y `Column` hace algo visible, los botones tienen personalidad propia (colores, padding, estado) y —por primera vez— **el `onClick` ejecuta código de verdad**: un contador que cambia la pantalla al pulsar.

**Duración estimada:** 2 horas de aula.

**Punto de partida:** proyecto de la práctica anterior con `@Preview` funcionando.

### 1. La chuleta: atributos de cada contenedor

Antes de escribir código, ten claro el mapa. `Row` y `Column` son gemelos espejados: lo que en uno es el eje principal, en el otro es el perpendicular.

| Atributo | En `Row` controla... | En `Column` controla... |
|----------|---------------------|------------------------|
| `modifier` | tamaño, márgenes, fondo... (igual en ambos) | ídem |
| `Arrangement.*` | **eje X** (horizontal, el principal) | **eje Y** (vertical, el principal) |
| `Alignment.*` | **eje Y** (el perpendicular) | **eje X** (el perpendicular) |

Valores de `Arrangement` (reparto del espacio en el eje principal):

| Valor | Qué hace | Se reconoce porque... |
|-------|----------|----------------------|
| `Start` / `Top` | pega los hijos al principio | es el valor por defecto |
| `Center` | los centra | hueco igual antes y después del grupo |
| `End` / `Bottom` | los pega al final | hueco todo al principio |
| `SpaceBetween` | primero y último a los extremos, resto repartido | el clásico navbar |
| `SpaceAround` | hueco alrededor de CADA hijo (los extremos tienen medio hueco) | los del medio tienen doble hueco |
| `SpaceEvenly` | hueco EXACTO entre todos, incluidos extremos | todo simétrico |
| `spacedBy(X.dp)` | solo separación fija entre hijos, sin centrar | práctico para listas |

Valores de `Alignment` (alineación en el perpendicular): `Top`, `CenterVertically`, `Bottom` (en `Row`) / `Start`, `CenterHorizontally`, `End` (en `Column`).

### 2. Enunciados

#### Ejercicio 1 — La Column maestra

Crea un composable `PracticaContenedores()` con una `Column` que:

1. Ocupe toda la pantalla Y deje 16dp de margen con los bordes (un solo `modifier` encadenando dos instrucciones).
2. Reparta a sus hijos verticalmente con `Arrangement.spacedBy(12.dp)`.
3. Centre a sus hijos horizontalmente con `Alignment.CenterHorizontally`.

Dentro mete: un `Text` de título con `fontSize = 22.sp` y un `Text` pequeño de 14.sp.

#### Ejercicio 2 — El laboratorio de Arrangement

Debajo, añade una `Row` que ocupe todo el ancho (`fillMaxWidth`) con tres `Text`: `"izq"`, `"centro"`, `"der"`. Ponle `horizontalArrangement = Arrangement.SpaceBetween` y añade encima un `Text` que diga qué configuración se está mostrando.

> **Para el experimentation:** cambia `SpaceBetween` por `SpaceAround`, `SpaceEvenly` y `Center` (uno cada vez) y haz captura de cada uno. ¿Sabrías reconocer cada modo con la pantalla delante sin mirar el código?

#### Ejercicio 3 — Botones con personalidad

Añade otra `Row` (esta con `SpaceEvenly`) con DOS botones:

**Botón ACEPTAR:**
- `onClick` que **sume 1** a una variable `pulsaciones` (estado con `remember { mutableIntStateOf(0) }` — se explica abajo)
- `colors` con `ButtonDefaults.buttonColors(containerColor = Color(0xFF2E7D32))` (verde)
- `contentPadding = PaddingValues(start = 20.dp, end = 20.dp, top = 12.dp, bottom = 12.dp)`
- `Text("ACEPTAR")` dentro

**Botón Reset:**
- `onClick` que ponga el contador a **0**
- `enabled = pulsaciones > 0` — el botón se pone GRIS y no responde cuando el contador ya está a cero
- `colors` rojo: `Color(0xFFB3261E)`

#### Ejercicio 4 — El contador visible (estado + Card)

1. Declara el estado arriba del todo del composable:
   ```kotlin
   var pulsaciones by remember { mutableIntStateOf(0) }
   ```
2. Añade una **`Card`** (el contenedor "etiqueta": un panel con fondo propio, esquinas redondeadas y sombra) entre el título y los botones:
   - `modifier = Modifier.fillMaxWidth()`
   - `colors` con `CardDefaults.cardColors(containerColor = Color(0xFF1F4E79))`
   - `elevation = CardDefaults.cardElevation(defaultElevation = 6.dp)` (la sombra que la "levanta")
   - Dentro, un `Text` blanco que muestre `"Has pulsado Aceptar $pulsaciones veces"` (con `modifier = Modifier.padding(12.dp)` y `color = Color.White`)
3. Pulsa ACEPTAR 3 veces → el texto de la Card debe cambiar SOLO. Eso es **recomposición**: al cambiar el estado, Compose vuelve a ejecutar el composable que lo lee.

#### ¿Qué es ese `remember { mutableIntStateOf }`?

Es la forma que tiene Compose de decir: *"recuerda este valor entre recomposiciones, y cuando cambie, re-dibuja quien lo esté usando"*. Lo veréis en detalle en la siguiente unidad; hoy lo usáis como receta: declarar → modificar dentro de un `onClick` → leerlo en un `Text`. El `$pulsaciones` dentro de un String es interpolación: incrusta el valor actual en el texto.

### 3. Diagramas: cómo debe quedar en el emulador

**Al arrancar (contador a 0, Reset desactivado en gris):**

```text
┌─────────────────────────────┐
│                             │
│ Práctica de contenedores    │  ← Text 22sp, centrado (CenterHorizontally)
│                             │
│ ┌─────────────────────────┐ │
│ │▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│ │  ← CARD azul marino (0xFF1F4E79),
│ │▓ Has pulsado Aceptar   ▓│ │     sombra elevation 6dp,
│ │▓ 0 veces               ▓│ │     texto blanco
│ └─────────────────────────┘ │
│                             │  ← 12dp de separación (spacedBy)
│      Prueba los botones:    │
│                             │
│   [ ACEPTAR ]  [ Reset ]    │  ← Row SpaceEvenly: mismo hueco
│        verde       gris     │     entre y alrededor de ambos.
│                             │     Reset GRIS: enabled = false
│                             │     porque pulsaciones == 0
│ — Arrangement.SpaceBetween —│
│ izq         centro        der│ ← Row SpaceBetween: izq pegado a la
│                             │    izquierda, der a la derecha,
│  (margen de 16dp en todo    │    centro en medio
│   el borde de la pantalla)  │
└─────────────────────────────┘
```

**Tras pulsar ACEPTAR 3 veces:**

```text
┌─────────────────────────────┐
│                             │
│ Práctica de contenedores    │
│                             │
│ ┌─────────────────────────┐ │
│ │▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│ │
│ │▓ Has pulsado Aceptar   ▓│ │  ← SOLO cambia este texto:
│ │▓ 3 veces               ▓│ │     recomposición al cambiar
│ └─────────────────────────┘ │     el estado
│                             │
│      Prueba los botones:    │
│                             │
│   [ ACEPTAR ]  [ Reset ]    │  ← Reset AHORA rojo y activo
│        verde       rojo     │     (enabled = pulsaciones > 0)
│                             │
│ — Arrangement.SpaceBetween —│
│ izq         centro        der│
│                             │
└─────────────────────────────┘
```

**Checklist visual antes de entregar:**

- [ ] El título está centrado horizontalmente (si sale a la izquierda, te falta el `Alignment` de la Column)
- [ ] La Card es azul marino con texto blanco y "flota" (sombra)
- [ ] Los dos botones caben en la misma línea con hueco igual entre ellos (`SpaceEvenly`)
- [ ] Reset arranca GRIS y se activa al primer clic en ACEPTAR
- [ ] El texto de la Card se actualiza al instante al pulsar (recomposición)
- [ ] izq/centro/der están pegados a sus extremos (`SpaceBetween`)

### 4. Solución

!!! warning "Antes de mirar"
    Intenta montarlo tú con los diagramas de arriba. La solución está **verificada compilando** (`compileDebugKotlin EXIT:0`), pero el valor está en pelearte con los atributos antes de leerla.

```kotlin
package org.example.project

import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.PaddingValues
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.padding
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Card
import androidx.compose.material3.CardDefaults
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableIntStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@Composable
fun PracticaContenedores() {
    // Estado: cuantas veces se ha pulsado Aceptar.
    // "remember" = recuerda el valor entre recomposiciones;
    // cuando cambia, todo composable que lo lea se vuelve a dibujar.
    var pulsaciones by remember { mutableIntStateOf(0) }

    // ══ EJERCICIO 1: COLUMN con todos sus atributos ══
    Column(
        modifier = Modifier
            .fillMaxSize()          // ocupa toda la pantalla...
            .padding(16.dp),        // ...y se lava las manos 16dp antes del borde
        verticalArrangement = Arrangement.spacedBy(12.dp),        // eje principal (Y): separacion fija entre hijos
        horizontalAlignment = Alignment.CenterHorizontally        // eje perpendicular (X): hijos centrados
    ) {
        Text("Práctica de contenedores", fontSize = 22.sp)

        // ══ EJERCICIO 4: CARD, el contenedor "etiqueta" ══
        Card(
            modifier = Modifier.fillMaxWidth(),                   // ancho completo (dentro del padding de la Column)
            colors = CardDefaults.cardColors(
                containerColor = Color(0xFF1F4E79)                // azul marino
            ),
            elevation = CardDefaults.cardElevation(
                defaultElevation = 6.dp                            // sombra: la "levanta" del fondo
            )
        ) {
            // La Card tambien usa el patron slot: dentro va lo que sea
            Text(
                text = "Has pulsado Aceptar $pulsaciones veces",  // $pulsaciones = interpolacion del estado
                color = Color.White,
                fontSize = 16.sp,
                modifier = Modifier.padding(12.dp)                // margen interno del texto
            )
        }

        Text("Prueba los botones:", fontSize = 14.sp)

        // ══ EJERCICIOS 2 y 3: ROW de botones con atributos ══
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceEvenly,      // eje principal (X): hueco identico entre todos
            verticalAlignment = Alignment.CenterVertically        // eje perpendicular (Y): misma altura
        ) {
            Button(
                onClick = {
                    // EJERCICIO 3: codigo real dentro del onClick
                    pulsaciones = pulsaciones + 1                 // tambien vale pulsaciones++
                },
                enabled = true,                                   // siempre clicable
                colors = ButtonDefaults.buttonColors(
                    containerColor = Color(0xFF2E7D32)            // verde
                ),
                contentPadding = PaddingValues(                   // aire interno del boton
                    start = 20.dp, end = 20.dp, top = 12.dp, bottom = 12.dp
                )
            ) {
                Text("ACEPTAR")
            }
            Button(
                onClick = { pulsaciones = 0 },                    // reinicia el contador
                enabled = pulsaciones > 0,                        // GRIS e inutilizable si el contador ya esta a 0
                colors = ButtonDefaults.buttonColors(
                    containerColor = Color(0xFFB3261E)            // rojo
                )
            ) {
                Text("Reset")
            }
        }

        // ══ EJERCICIO 2: laboratorio de Arrangement ══
        Text("— Arrangement.SpaceBetween —", fontSize = 12.sp)
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceBetween,     // extremos pegados, hueco en medio
            verticalAlignment = Alignment.CenterVertically
        ) {
            Text("izq")
            Text("centro")
            Text("der")
        }
    }
}
```

**Notas sobre la solución:**

- **Un `modifier` puede encadenar varias instrucciones**: `Modifier.fillMaxSize().padding(16.dp)` se lee de izquierda a derecha ("ocupa todo... y luego respeta 16dp por dentro"). El ORDEN importa: invertirlo cambia el resultado visible.
- **`enabled` es la atributo "estado" del botón**: con `false` se ve gris y no responde. Aquí lo conectamos al estado (`pulsaciones > 0`), así que Compose lo re-evalúa solo en cada recomposición.
- **La `Card` también es un slot**: dentro acepta cualquier composable, igual que el `Button`. Los contenedores de Compose comparten ese patrón.
- **`0xFF1F4E79`** es color ARGB: `FF` = opacidad total, y `1F4E79` el azul. Cambia los dos últimos pares para experimentar.

**Errores típicos que vas a cometer (y su familia):**

| Si te sale | Causa | Familia |
|---|---|---|
| `Unresolved reference 'Card'` | falta `import androidx.compose.material3.Card` (Alt+Enter) | B — Import |
| `Unresolved reference 'mutableIntStateOf'` | falta el import de `androidx.compose.runtime.mutableIntStateOf` | B — Import |
| `Unresolved reference 'dp'` / `'sp'` | faltan `androidx.compose.ui.unit.dp` y `.sp` | B — Import |
| `No value passed for parameter 'onClick'` | te olvidaste del parámetro obligatorio | C — Parámetro |
| El contador suma pero la Card no cambia | el `Text` que lee `pulsaciones` no está en el composable que se recompon... revisa que el `by remember` esté DENTRO del composable | Silenciosa |
| Botones pegados en la esquina | te falta el `Arrangement`/`Alignment` del Row — compila, pero se ve mal | Silenciosa |

### 5. Entrega

- Captura del emulador al arrancar (contador a 0, Reset gris) y tras 3 pulsaciones (contador a 3, Reset rojo).
- Captura del laboratorio de Arrangement con `SpaceBetween` y con otros dos modos a tu elección, etiquetadas.
- El código de tu `PracticaContenedores.kt`.
- **Pregunta para responder en 2 líneas:** ¿por qué el texto de la Card se actualiza solo si nadie lo vuelve a pintar a mano?
