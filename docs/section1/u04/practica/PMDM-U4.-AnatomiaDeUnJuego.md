---
title: "Práctica 4.1 - Anatomía de un juego (análisis y prototipo)"
description: "Analizar un juego existente por dentro (bloques funcionales y escena) y construir un prototipo mínimo en Godot con capturas propias."
authors:
    - Ismael Velasco
date: 2026-10-25
icon: "material/tools"
permalink: /pmdm/unidad4/practica
categories:
    - PMDM
tags:
    - PMDM
    - Godot
    - Juegos
---
# Práctica 4.1 - Anatomía de un juego

**UD 4 · RA4 (CE a, b, e, f) · RD 405/2023 · 2 sesiones de 55 min · entregable: informe + prototipo + capturas**

No se trata de hacer un juego: se trata de **abrir uno y contar cómo está hecho**, y luego demostrar que sabes montar la estructura mínima en un motor. El juego que analices es elección tuya (uno de móvil, uno de navegador, uno de consola), pero tiene que ser **conocido y verificable**.

## Qué tiene que tener (checklist de entrega)

| # | Requisito | Qué demuestra | Apartado |
|---|---|---|---|
| 1 | Ficha del juego: nombre, año, motor (si se conoce), plataformas y género | análisis del entorno | 4.3 |
| 2 | Los **bloques funcionales**: entrada, lógica, física, render, audio y HUD | CE e) | 4.2 |
| 3 | **Croquis de una pantalla** del juego con los elementos espaciales señalados | CE f) | 4.1 |
| 4 | El **árbol de escena equivalente**: cómo se montaría esa pantalla con nodos | CE a) | 4.1 |
| 5 | Prototipo en Godot: escena 2D con un nodo que se mueve con las flechas | CE a) | 4.1 |
| 6 | Prototipo: **HUD con FPS y posición** funcionando | CE b) | 4.2 |
| 7 | Captura del prototipo ejecutándose y **volcado del árbol** por consola | CE a) | 4.4 |
| 8 | Informe con las decisiones y **un error real** que hayas tenido, con su solución | CE f) | 4.4 |

## Orden de trabajo (los minutos cuentan)

**Sesión 1 (55 min) — análisis**
1. (10) Elige el juego y rellena la ficha (requisito 1).
2. (20) Bloques funcionales: para cada bloque, **qué hace** y **qué se ve** en pantalla (requisito 2).
3. (15) Croquis de una pantalla: jugador, obstáculos, HUD, cámara (requisito 3).
4. (10) Árbol de escena equivalente: raíz y nodos hijos con su tipo (requisito 4).

**Sesión 2 (55 min) — prototipo**
5. (10) Proyecto nuevo en Godot y escena 2D con fondo y jugador.
6. (20) Movimiento con flechas y colisión con un muro (requisitos 5).
7. (15) HUD con FPS y posición (requisito 6).
8. (10) Captura del prototipo, volcado del árbol y cierre del informe (requisitos 7 y 8).

## Entregable

- **Informe** (PDF o Markdown) con los ocho requisitos, cada uno con su captura o su tabla.
- **Proyecto** de Godot comprimido (`.zip` sin la carpeta `.godot`).
- Capturas con nombre claro: `01-ficha.png`, `02-croquis.png`, `03-arbol.png`, `04-prototipo.png`, `05-error.png`.

## Criterios de corrección (sobre 10)

| Apartado | Peso |
|---|---|
| Ficha y bloques funcionales bien identificados | 2,5 |
| Croquis y árbol de escena equivalente coherentes | 2,5 |
| Prototipo ejecutable con movimiento y colisión | 2 |
| HUD con FPS y posición | 1 |
| Volcado del árbol por consola | 1 |
| Informe con el error real y su solución | 1 |

**Descuentos automáticos:** captura que no se corresponde con el proyecto entregado (−1); prototipo que no arranca con F5 (−1); informe sin el error real (−0,5); bloques funcionales copiados de una web sin adaptarlos al juego elegido (−1).

## Consejos (y una advertencia)

- Elige un juego **pequeño y conocido**: analizar un battle royale es más difícil que analizar un Pong, y se puntúa igual.
- Los **bloques funcionales** no son "cosas que se ven": son responsabilidades. El HUD no es un bloque de render, es interfaz.
- Para el árbol, no hace falta acertar los nombres del motor original: hace falta que la **jerarquía tenga sentido**.
- Guarda el proyecto **sin la carpeta `.godot`** al comprimir: esa carpeta se regenera y pesa mucho.
- **Advertencia:** si el prototipo no se ejecuta en tu equipo, no se corrige. Pruébalo antes de entregar, no después.

## Bibliografía y documentación oficial

- Godot Engine — documentación oficial: [docs.godotengine.org](https://docs.godotengine.org/en/stable/).
- Godot Engine — nodos y escenas: [docs.godotengine.org — Nodes and scenes](https://docs.godotengine.org/en/stable/getting_started/step_by_step/nodes_and_scenes.html).
- Godot Engine — señales: [docs.godotengine.org — Signals](https://docs.godotengine.org/en/stable/getting_started/step_by_step/signals.html).
- Godot Engine — descargas: [godotengine.org/download](https://godotengine.org/download/).
- Unity — manual: [docs.unity3d.com/Manual](https://docs.unity3d.com/Manual/index.html).
- Unreal Engine — documentación: [dev.epicgames.com — Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine).
- libGDX — wiki: [libgdx.com/wiki](https://libgdx.com/wiki/).
- Phaser — documentación: [docs.phaser.io](https://docs.phaser.io/).
- Módulo 0489 — RD 405/2023 (BOE): [BOE-A-2023-13221](https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221).
