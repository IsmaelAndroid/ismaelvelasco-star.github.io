---
title: "UD 1 - P5: Imágenes y recursos (1 h)"
description: "Tercera práctica de la trilogía: meter un PNG en res/drawable, pintarlo con Image, y mostrarlo/ocultarlo con un botón toggle. Sesión de 1 hora."
summary: "Tu primer recurso: imagen en res/drawable, painterResource, composición condicional con if y un botón que alterna."
authors:
    - Ismael Velasco
date: 2026-10-06
icon: "material/file-document-edit"
permalink: /di/unidad1/p5
categories:
    - DI
tags:
    - DI
    - Compose
    - Kotlin
    - Image
    - Recursos

toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

# Práctica 5: Imágenes y recursos (1 h)

Cierre de la trilogía. Hasta ahora todo era texto y color; hoy estrenas un **recurso real**: una imagen que vive en una carpeta del proyecto (no en un String) y que el usuario puede mostrar y ocultar con botones. Es la primera vez que el estado decide **qué composables existen** (no solo cómo se ven).

**Punto de partida:** la app de la P4 funcionando (contador + Card + filas).

### 1. La chuleta: recursos e Image

**Carpetas de `res/`:**

| Carpeta | Qué contiene |
|---------|--------------|
| `res/drawable/` | imágenes (PNG, JPG, WebP, vectores XML) |
| `res/values/` | strings.xml, colors.xml, themes... |
| `res/mipmap/` | iconos de la app (launcher) |

**Reglas del nombre del recurso:** SOLO minúsculas, números y guión bajo. `demo_botones` ✅ / `demo-botones` ❌ / `DemoBotones` ❌. Sin espacios. Sin tildes.

**Cómo se referencia desde código:** `R.drawable.nombre_sin_extension` — la clase `R` la genera Android automáticamente con TODOS los recursos. Si acabas de copiar un archivo y `R` sale en rojo: **Build → Clean Project** y compila; se regenera sola.

**El composable:** `Image(painter = painterResource(id), contentDescription = "...", modifier = ...)`. El `contentDescription` es para lectores de pantalla (accesibilidad): siempre se rellena (o se pone `null` si la imagen es puramente decorativa).

### 2. Enunciados

#### Ejercicio 1 — Meter el recurso (10 min)

1. Consigue un PNG pequeño (un logo, un meme, un icono — cualquier cosa vale).
2. Cópialo en `app/src/main/res/drawable/` (en Android Studio: arrastra el archivo a la carpeta, o clic derecho en `res` → **New → Android Resource File**).
3. Nómbralo siguiendo las reglas (por ejemplo `mi_imagen.png`).
4. Verifica en el código que Android ya lo ve: escribe `R.drawable.` y mira el autocompletado — debe aparecer tu imagen.

#### Ejercicio 2 — Pintarla (15 min)

Crea un composable `PracticaImagen()` con una Column (la estructura de siempre) y dentro:

```kotlin
Image(
    painter = painterResource(R.drawable.mi_imagen),
    contentDescription = "Lo que represente tu imagen",
    modifier = Modifier.size(240.dp)
)
```

Añade un `Text` de título `"Galería"`. Comprueba en el emulador que la imagen se ve a 240dp.

#### Ejercicio 3 — Mostrar / Ocultar (20 min)

1. Declara el estado:
   ```kotlin
   var imagenVisible by remember { mutableStateOf(true) }
   ```
2. Rodea la `Image` con un `if`:
   ```kotlin
   if (imagenVisible) {
       Image(...)
   } else {
       Text("(imagen oculta)")
   }
   ```
3. Añade una `Row` con `SpaceEvenly` y TRES botones:
   - `"Mostrar"` → `onClick = { imagenVisible = true }`
   - `"Ocultar"` → `onClick = { imagenVisible = false }`
   - `"Alternar"` → `onClick = { imagenVisible = !imagenVisible }` y de etiqueta `Text(if (imagenVisible) "Ocultar" else "Mostrar")`

El tercero es un **toggle**: invierte el valor (`!imagenVisible`) y su propia etiqueta cambia sola porque también se recomposición. Un botón, dos funciones.

#### Ejercicio 4 — Tres a la vez (15 min)

Añade DOS imágenes más (cualquier PNG con nombres válidos distintos) y un selector: tres botones en una Row (`"Foto 1"`, `"Foto 2"`, `"Foto 3"`) que cambien un estado `var fotoActual by remember { mutableIntStateOf(1) }`. Con un `when (fotoActual)` decide cuál de las tres `Image` se pinta:

```kotlin
when (fotoActual) {
    1 -> Image(painterResource(R.drawable.foto1), ...)
    2 -> Image(painterResource(R.drawable.foto2), ...)
    3 -> Image(painterResource(R.drawable.foto3), ...)
}
```

Ya tienes una galería de 3 fotos con selector: la misma idea que un carrusel.

### 3. Diagrama: cómo debe quedar en el emulador

**Al arrancar (foto 1, visible):**

```text
┌─────────────────────────────┐
│           Galería           │
│                             │
│   ┌─────────────────────┐   │
│   │                     │   │
│   │    (tu foto 1)      │   │  ← Image 240dp pintada por
│   │                     │   │     painterResource(...)
│   └─────────────────────┘   │
│                             │
│  [Mostrar][Ocultar][Ocultar]│  ← Row SpaceEvenly; el 3º
│                             │     dice "Ocultar" porque
│                             │     ahora SE VE la imagen
│   [Foto 1] [Foto 2] [Foto 3]│  ← selector (ejercicio 4)
│      ▔▔▔▔                   │
└─────────────────────────────┘
```

**Tras pulsar Ocultar (o el toggle):**

```text
┌─────────────────────────────┐
│           Galería           │
│                             │
│      (imagen oculta)        │  ← el Text ocupa el sitio
│                             │     de la Image: mismo hueco
│  [Mostrar][Ocultar][Mostrar]│  ← el toggle CAMBIÓ su
│                             │     etiqueta él solo
│   [Foto 1] [Foto 2] [Foto 3]│
│                             │
└─────────────────────────────┘
```

**Checklist antes de terminar la hora:**

- [ ] El PNG está en `res/drawable/` con nombre válido (el autocompletado de `R.drawable.` lo encuentra)
- [ ] Al pulsar Ocultar la imagen desaparece AL INSTANTE
- [ ] El botón toggle cambia su propio texto según el estado
- [ ] El selector pinta la foto que toca (ejercicio 4)
- [ ] `contentDescription` rellenado en todas las `Image`

### 4. Entrega

- Captura con la imagen visible y otra oculta (se debe ver el toggle cambiado).
- Captura del selector con las 3 fotos (una captura por foto seleccionada).
- Captura de la carpeta `res/drawable` con tus PNG dentro.
- **Pregunta (2 líneas):** ¿por qué `R.drawable.mi_imagen` no lleva la extensión `.png`?

> La solución está en el [solucionario de prácticas](/di/unidad1/solucionario-practicas/) — pélea antes con los diagramas de arriba.
