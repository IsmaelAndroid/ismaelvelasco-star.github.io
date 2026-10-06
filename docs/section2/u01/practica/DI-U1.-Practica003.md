---
title: "UD 1 - P3: Column, botones y contador (1 h)"
description: "Primera práctica de la trilogía de contenedores: Column con todos sus atributos, botones personalizados y un contador con estado. Sesión de 1 hora."
summary: "Column maestra, botones con colores y padding, y el primer estado con remember: un contador que se actualiza solo."
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
    - Column
    - Button

toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

# Práctica 3: Column, botones y contador (1 h)

Primera práctica de una trilogía de tres sesiones de 1 hora: **Column** hoy, **Row** en la P4 y **recursos/imágenes** en la P5. Todo con la misma app como hilo conductor: un contador de pulsaciones.

**Lo que vas a tocar:** los tres atributos de `Column`, los atributos de `Button` (colores, padding, `enabled`) y tu primer **estado** (`remember`): una variable que al cambiar re-dibuja la pantalla sola.

### 1. La chuleta: Column

| Atributo | Qué controla |
|----------|--------------|
| `modifier` | tamaño, márgenes, fondo... |
| `verticalArrangement = Arrangement.X` | **eje Y** (el principal de una Column): cómo se reparten los hijos de arriba a abajo |
| `horizontalAlignment = Alignment.X` | **eje X** (el perpendicular): dónde se alinean los hijos |

Valores de `Arrangement` (reparto en el eje principal): `Top` (por defecto), `Center`, `Bottom`, `SpaceBetween`, `SpaceAround`, `SpaceEvenly`, `spacedBy(X.dp)`.

Valores de `Alignment` para el perpendicular de una Column: `Start`, `CenterHorizontally`, `End`.

### 2. Enunciados

#### Ejercicio 1 — La Column maestra (15 min)

Crea un composable `PracticaContador()` con una `Column` que:

1. Ocupe toda la pantalla y deje **16dp de margen** con los bordes (un solo `modifier` encadenando `fillMaxSize()` y `padding(16.dp)`).
2. Separe a sus hijos verticalmente con `Arrangement.spacedBy(12.dp)`.
3. Centre a sus hijos horizontalmente con `Alignment.CenterHorizontally`.

Dentro mete: un `Text` de título `"Contador de pulsaciones"` con `fontSize = 22.sp`.

#### Ejercicio 2 — Botones con personalidad (20 min)

Añade dos botones DENTRO de la Column (saldrán apilados en vertical, ya los pondremos en fila en la P4):

**Botón SUMAR:**
- `onClick` que sume 1 a una variable `pulsaciones`
- `colors` con `ButtonDefaults.buttonColors(containerColor = Color(0xFF2E7D32))` (verde)
- `contentPadding = PaddingValues(start = 24.dp, end = 24.dp, top = 12.dp, bottom = 12.dp)`

**Botón REINICIAR:**
- `onClick` que ponga el contador a 0
- `enabled = pulsaciones > 0` — se ve GRIS y no responde si el contador ya está a cero
- `colors` rojo: `Color(0xFFB3261E)`

#### Ejercicio 3 — El contador visible (15 min)

1. Declara el estado arriba del todo del composable:
   ```kotlin
   var pulsaciones by remember { mutableIntStateOf(0) }
   ```
2. Añade un `Text` grande (`fontSize = 48.sp`) entre el título y los botones que muestre el valor: `Text("$pulsaciones")`.
3. Pulsa SUMAR 3 veces → el número cambia SOLO. Eso es **recomposición**: al cambiar el estado, Compose vuelve a ejecutar el composable que lo lee.

#### Ejercicio 4 — Comprobación visual (10 min)

Sin tocar más código, responde en un comentario del archivo (`//`):

1. ¿Qué pasa si quitas el `horizontalAlignment` de la Column? ¿Compila? ¿Cómo se ve?
2. ¿Qué pasa si cambias `spacedBy(12.dp)` por `Arrangement.Center`? ¿Dónde queda el grupo de hijos?
3. ¿Por qué REINICIAR está gris al arrancar y se activa con la primera pulsación?

### 3. Diagrama: cómo debe quedar en el emulador

**Al arrancar (contador a 0):**

```text
┌─────────────────────────────┐
│                             │
│  Contador de pulsaciones    │  ← Text 22sp, centrado
│                             │
│             0               │  ← Text 48sp (el estado)
│                             │
│        [ SUMAR ]            │  ← verde, contentPadding
│                             │     ancho (24dp laterales)
│        [ REINICIAR ]        │  ← GRIS: enabled = false
│                             │     porque pulsaciones == 0
│                             │
│  (margen de 16dp en todo    │
│   el borde de la pantalla)  │
└─────────────────────────────┘
```

**Tras pulsar SUMAR 3 veces:**

```text
┌─────────────────────────────┐
│                             │
│  Contador de pulsaciones    │
│                             │
│             3               │  ← SOLO cambia este número
│                             │
│        [ SUMAR ]            │
│                             │
│        [ REINICIAR ]        │  ← AHORA rojo y clicable
│                             │
└─────────────────────────────┘
```

**Checklist antes de terminar la hora:**

- [ ] El título y el número están centrados horizontalmente
- [ ] SUMAR es verde con aire interior ancho; REINICIAR arranca gris
- [ ] El número cambia al instante al pulsar (recomposición)
- [ ] Los tres comentarios del ejercicio 4 están escritos en el archivo

### 4. Entrega

- Captura del emulador al arrancar (0, REINICIAR gris) y tras 3 pulsaciones (3, REINICIAR rojo).
- El código de tu `PracticaContador.kt` con los comentarios del ejercicio 4.

> La solución está en el [solucionario de prácticas](/di/unidad1/solucionario-practicas/) — pélea antes con los diagramas de arriba.
