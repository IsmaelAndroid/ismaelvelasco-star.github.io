---
title: "UD 1 - P4: Row y Card (1 h)"
description: "Segunda práctica de la trilogía: Row con todos sus atributos, el laboratorio de Arrangement y la Card como contenedor tipo etiqueta. Sesión de 1 hora."
summary: "Row en todas sus variantes (SpaceBetween, SpaceEvenly...), columnas dentro de filas, y la Card con fondo y sombra."
authors:
    - Ismael Velasco
date: 2026-10-06
icon: "material/file-document-edit"
permalink: /di/unidad1/p4
categories:
    - DI
tags:
    - DI
    - Compose
    - Kotlin
    - Row
    - Card

toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

# Práctica 4: Row y Card (1 h)

Segunda sesión de la trilogía. En la P3 apilaste botones en vertical con `Column`; hoy los pones **en fila** con `Row`, descubres que `Row` y `Column` son gemelos espejados (lo que en uno es el eje principal, en el otro es el perpendicular) y estrenas la **`Card`**: un contenedor "etiqueta" con fondo propio, esquinas redondeadas y sombra.

**Punto de partida:** la app del contador de la P3 funcionando.

### 1. La chuleta: Row (el espejo de Column)

| Atributo | En `Row` controla... | En `Column` controlaba... |
|----------|---------------------|--------------------------|
| `modifier` | tamaño, márgenes (igual) | ídem |
| `horizontalArrangement` | **eje X** (principal) | era el perpendicular |
| `verticalAlignment` | **eje Y** (perpendicular) | era el principal |

**Regla mnemotécnica:** el `Arrangement` siempre manda en el eje en el que el contenedor "fluye" (X en Row, Y en Column); el `Alignment` en el cruzado.

Valores de `Arrangement`: `Start` (defecto), `Center`, `End`, `SpaceBetween`, `SpaceAround`, `SpaceEvenly`, `spacedBy(X.dp)`.

Valores de `Alignment` para el perpendicular de un Row: `Top`, `CenterVertically`, `Bottom`.

### 2. Enunciados

#### Ejercicio 1 — Botones en fila (15 min)

Pasa los dos botones de la P3 (SUMAR y REINICIAR) a un `Row`:

1. `Row` con `modifier = Modifier.fillMaxWidth()` (todo el ancho que le deja la Column padre).
2. `horizontalArrangement = Arrangement.SpaceEvenly` — hueco idéntico entre y alrededor de ambos.
3. `verticalAlignment = Alignment.CenterVertically`.

Los botones deben quedar en la MISMA línea, separados y centrados.

#### Ejercicio 2 — El laboratorio de Arrangement (20 min)

Añade debajo una fila de tres `Text` (`"izq"`, `"centro"`, `"der"`) con `horizontalArrangement = Arrangement.SpaceBetween` y un `Text` encima que etiquete el modo.

Cambia `SpaceBetween` por `SpaceAround`, `SpaceEvenly` y `Center` (uno cada vez) y haz **captura de cada modo**. Regla para reconocerlos a simple vista:

| Modo | Se reconoce porque... |
|------|----------------------|
| `SpaceBetween` | primero y último PEGADOS a los extremos, hueco solo en medio |
| `SpaceAround` | todos con hueco alrededor; los extremos tienen MEDIO hueco (los del medio, doble) |
| `SpaceEvenly` | hueco EXACTAMENTE igual en todos los huecos, incluidos los extremos |
| `Center` | el grupo junto y centrado; todo el hueco fuera, repartido en 2 trozos |

#### Ejercicio 3 — La Card del marcador (15 min)

Convierte el `Text` del número (el de 48sp) en una **`Card`**:

```kotlin
Card(
    modifier = Modifier.fillMaxWidth(),
    colors = CardDefaults.cardColors(containerColor = Color(0xFF1F4E79)),
    elevation = CardDefaults.cardElevation(defaultElevation = 6.dp)
) {
    Text("$pulsaciones", fontSize = 48.sp, color = Color.White,
         modifier = Modifier.fillMaxWidth(), textAlign = TextAlign.Center)
}
```

La `Card` es el contenedor "etiqueta" de Material: fondo propio, esquinas redondeadas y **sombra** (`elevation`) que la "levanta" del fondo. Como `Button` y como `Row`, usa el patrón slot: dentro va lo que tú quieras.

#### Ejercicio 4 — Anidar: una mini-toolbar (10 min)

Añade al final una `Row` con `SpaceBetween` que contenga DOS `Column` (una con `Text("Puntos")` + `Text("$pulsaciones")` pequeño, otra con `Text("Sesión")` + `Text("UD1")` pequeña). Objetivo: ver que un hijo de un Row puede ser un contenedor con sus propios hijos — **anidar es gratis**.

### 3. Diagrama: cómo debe quedar en el emulador

**Al arrancar (contador a 0):**

```text
┌─────────────────────────────┐
│  Contador de pulsaciones    │
│                             │
│ ┌─────────────────────────┐ │
│ │▓▓▓▓▓▓▓▓▓▓▓ 0 ▓▓▓▓▓▓▓▓▓▓▓│ │  ← CARD azul marino, texto
│ └─────────────────────────┘ │     blanco 48sp, sombra
│                             │
│    [ SUMAR ] [ REINICIAR ]  │  ← Row SpaceEvenly: misma
│      verde       gris       │     línea, hueco simétrico
│                             │
│ — SpaceBetween —            │
│ izq         centro        der│ ← extremos pegados
│                             │
│ Puntos            Sesión    │  ← toolbar (Row con 2 Column)
│   0                 UD1     │
└─────────────────────────────┘
```

**Checklist antes de terminar la hora:**

- [ ] SUMAR y REINICIAR en la misma línea, con hueco igual a ambos lados
- [ ] 4 capturas del laboratorio etiquetadas con su modo
- [ ] La Card es azul con número blanco centrado y "flota" (sombra)
- [ ] La toolbar inferior tiene 2 Column dentro de 1 Row

### 4. Entrega

- Las 4 capturas del laboratorio de Arrangement etiquetadas (SpaceBetween / SpaceAround / SpaceEvenly / Center).
- Captura de la app completa al arrancar y tras 5 pulsaciones.
- **Pregunta (2 líneas):** ¿por qué en el Row de botones usamos `SpaceEvenly` y en el laboratorio `SpaceBetween`? ¿Qué los diferencia visualmente?

> La solución está en el [solucionario de prácticas](/di/unidad1/solucionario-practicas/) — pélea antes con los diagramas de arriba.
