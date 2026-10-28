---
title: "Tema 4.4 - Taller: primer proyecto, ejecutar y depurar"
summary: "Taller guiado para montar el proyecto desde cero, ejecutarlo, leer los errores de verdad y medir los FPS sin adivinar."
series: "Programación de Dispositivos Móviles"
series_order: 4
section: "4. Selección y prueba de motores de juegos"
author: Ismael Velasco
date: 2026-10-25
weight: 4
---
# Tema 4.4 - Taller: primer proyecto, ejecutar y depurar

Último apartado de teoría: lo montas tú. Sin capturas nuevas que copiar, con los pasos y los errores reales que aparecen cuando se hace por primera vez.

<div style="background-color:#a5d6a7; border-radius:8px; padding:10px 14px; margin:14px 0;">
<strong>🟢 Leyenda de repaso obligatorio:</strong> los textos con <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">este fondo verde</span> son imprescindibles y entran en todas las evaluaciones del módulo.</div>

## 1. Lo que hay que conseguir al terminar

- [ ] Un proyecto propio que arranque con **F5**.
- [ ] Una escena 2D con un nodo que se mueve con las flechas.
- [ ] Un HUD que muestre **FPS y posición**.
- [ ] Una escena 3D con cámara y luz.
- [ ] Un error provocado a propósito y **explicado**.

## 2. Los ocho pasos

1. **Descarga** Godot 4.7.2 y descomprime el `.zip` (no hay instalador).
2. Abre el ejecutable y, en el gestor de proyectos, pulsa **Crear** (nuevo proyecto). **Importar** es solo para abrir un proyecto que ya existe, como el `IsmaelArcade` del aula.
3. Ponle nombre al proyecto y elige una carpeta vacía (o pulsa **Crear carpeta**).
4. Elige el renderizador **Compatibilidad** y pulsa **Crear y editar**; el motor generará `project.godot`.
5. En el panel **Escena**, botón **+** → busca `Node2D` → **Crear**.
6. Añade los hijos con el botón **+** sobre el nodo raíz: `ColorRect` (fondo), `CharacterBody2D` (jugador), `Camera2D`, `CanvasLayer` con `Label` (HUD).
7. Selecciona el nodo del jugador y pulsa el icono de **script** → crea `jugador.gd`.
8. Guarda la escena (`Ctrl+S`) y pulsa **F5**.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">F5 ejecuta el proyecto entero; F6 ejecuta solo la escena que tienes abierta. Cuando algo "no sale", casi siempre es que estabas pulsando F6 en otra escena.</span>

## 3. Ejecutar desde la terminal

El editor no es la única forma. Desde una consola, dentro de la carpeta del proyecto:

```bash
godot --path . escena_2d.tscn
```

Y sin ventana, para comprobar que una escena carga bien:

```bash
godot --headless --path . --script dump_arbol.gd
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`--headless` no abre ventana: es la forma de comprobar escenas y scripts en una máquina sin pantalla, y la que se usa en un servidor de integración.</span>

## 4. Leer un error de verdad

Este error salió en este mismo proyecto al escribir el script que vuelca el árbol:

```text
SCRIPT ERROR: Parse Error: Cannot infer the type of "escena" variable because the value doesn't have a set type.
   at: GDScript::reload (res://dump_arbol.gd:5)
ERROR: Failed to load script "res://dump_arbol.gd" with error "Parse error".
```

Se lee de abajo arriba: **falló la carga del script** porque hay un **error de análisis** en la línea 5, y el motivo es que no puede deducir el tipo de la variable. La causa: `load()` devuelve un `Resource` genérico y `:=` intenta deducir el tipo.

La solución fue declarar los tipos a mano:

```gdscript
var empaquetada: PackedScene = load("res://escena_2d.tscn")   # tipo explícito
var escena: Node = empaquetada.instantiate()                  # tipo explícito
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un error de análisis (Parse Error) no llega a ejecutarse: el script no carga. Se arregla en la línea que indica el mensaje, no "tocando cosas" hasta que arranque.</span>

## 5. Errores que van a aparecer (y qué son)

| Mensaje o síntoma | Qué significa | Solución |
|---|---|---|
| `Cannot infer the type of ...` | falta el tipo en una variable con `:=` | declara el tipo (`var x: Node = ...`) |
| `Failed to request display timeout override` | aviso del sistema sobre la pantalla | **no es un error**: se ignora |
| Pulsas F5 y no pasa nada | `run/main_scene` no apunta a una escena válida | revisa `project.godot` |
| El jugador atraviesa el muro | moviste `position` a mano | usa `move_and_slide()` |
| En 3D no se ve nada | falta `Camera3D` o falta luz | añade cámara y `DirectionalLight3D` |
| Todo va a tirones | el HUD o la lógica se ejecutan de más | mide con `Engine.get_frames_per_second()` |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El aviso de "display timeout override" sale siempre en Windows y no rompe nada: aprender a distinguir aviso de error es parte de depurar.</span>

## 6. Medir en vez de opinar

El HUD del proyecto muestra los FPS reales. Se obtienen así:

```gdscript
Engine.get_frames_per_second()      # fotogramas por segundo reales
```

Con eso se comparan dos situaciones y se decide con datos:

| Situación | FPS esperados | Qué mirar |
|---|---|---|
| Escena 2D con 6 nodos | 60 estables | si baja, hay lógica en el sitio equivocado |
| Escena 3D con 5 nodos | 60 estables | si baja, la GPU o la luz están cargando de más |
| Con el editor abierto a la vez | puede bajar | el editor también consume recursos |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un juego "va mal" cuando los FPS bajan de forma sostenida, no cuando alguien lo dice. El HUD es la medida y la captura es la prueba.</span>

## 7. Lo que tienes que llevarte de este apartado

- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">F5 ejecuta el proyecto; F6, solo la escena abierta.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`--headless` comprueba escenas sin abrir ventana.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un `Parse Error` se lee en la línea que indica y se arregla ahí.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Aviso no es error: `display timeout override` se ignora.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Los FPS se miden con `Engine.get_frames_per_second()`, no se suponen.</span>

## Bibliografía y documentación oficial

- Godot Engine — documentación oficial: [docs.godotengine.org](https://docs.godotengine.org/en/stable/).
- Godot Engine — línea de comandos: [docs.godotengine.org — Command line tutorial](https://docs.godotengine.org/en/stable/tutorials/editor/command_line_tutorial.html).
- Godot Engine — GDScript y tipos: [docs.godotengine.org — GDScript](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/index.html).
- Godot Engine — depuración: [docs.godotengine.org — Debugging](https://docs.godotengine.org/en/stable/tutorials/scripting/debug/index.html).
- Godot Engine — descargas oficiales: [godotengine.org/download](https://godotengine.org/download/).
- Módulo 0489 — Real Decreto 405/2023 (BOE): [BOE-A-2023-13221](https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221).

## Presentación

- [PM-U4 - Motores de juegos](https://revilofe.github.io/slides/section1-pmdm/PM-U4.-MotoresDeJuegos.html)
