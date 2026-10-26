---
title: "Unidad 6 - Desarrollo de juegos 2D y 3D"
summary: "RA5: lógica del juego, objetos y escenas, físicas, materiales, sonido, cámara e iluminación, y despliegue en Android."
series: "Programación de Dispositivos Móviles"
series_order: 6
section: "5. Desarrollo de juegos 2D y 3D"
author: Ismael Velasco
date: 2026-10-25
weight: 6
---
# Unidad 6 - Desarrollo de juegos 2D y 3D

Última unidad del módulo. Aquí se cierra el resultado de aprendizaje **RA5**: construir un juego 2D y una escena 3D con un motor, y llevarlo a un móvil.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Esta unidad cierra el módulo: todo lo de las unidades 1 a 5 (entorno, datos, multimedia y motores) se usa aquí a la vez.</span>

## Recorrido de la unidad

| Apartado | Título | Horas | Criterios del RA5 |
|---|---|---|---|
| 6.1 | Del prototipo al juego: lógica, estados y objetos | 2 | a, b, c |
| 6.2 | Juego 2D: movimiento, colisiones y puntuación | 2,5 | b, c, e, f |
| 6.3 | Juego 3D: materiales, físicas, cámara e iluminación | 2,5 | d, e, g |
| 6.4 | Pruebas, optimización y despliegue en móvil | 1 | h, i, j |
| — | **Práctica: Mi primer juego** | 2 | a – j |
| | **Total** | **10** | |

## Teoría

- [Tema 6.1 - Del prototipo al juego: lógica, estados y objetos](teoria/PMDM-U6.1.-DelPrototipoAlJuego.md)
- [Tema 6.2 - Juego 2D: movimiento, colisiones y puntuación](teoria/PMDM-U6.2.-Juego2DMovimientoColisiones.md)
- [Tema 6.3 - Juego 3D: materiales, físicas, cámara e iluminación](teoria/PMDM-U6.3.-Juego3DMaterialesFisicas.md)
- [Tema 6.4 - Pruebas, optimización y despliegue en móvil](teoria/PMDM-U6.4.-PruebasOptimizacionYDespliegue.md)

## Presentaciones

* [PM-U6 - Juegos 2D y 3D](https://revilofe.github.io/slides/section1-pmdm/PM-U6.-Juegos2DY3D.html)

## Práctica evaluable

- [Práctica UD6 - Mi primer juego: recoge y gana](practica/PMDM-U6.-MiPrimerJuego.md)

## Proyecto de la unidad

| Elemento | Detalle |
|---|---|
| Nombre | **IsmaelJuego** |
| Motor | Godot 4.7.2 (renderizador `gl_compatibility`) |
| Escena 2D | `juego2d.tscn` — jugador, 3 monedas, HUD y sonido |
| Escena 3D | `fisica3d.tscn` — caja con `RigidBody3D`, suelo, cámara y luz |
| Scripts | `juego2d.gd` y `fisica3d.gd` |
| Sonido | `sonido.wav` (0,15 s, generado con `ffmpeg`) |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Todo el código de esta unidad está ejecutado en Godot 4.7.2 y las capturas salen del proyecto real, no de ejemplos copiados.</span>
