---
title: "Práctica UD6 - Mi primer juego: recoge y gana"
summary: "Práctica evaluable del RA5: juego 2D propio con colisiones, marcador, sonido y exportación a Android, con rúbrica sobre 10."
series: "Programación de Dispositivos Móviles"
series_order: 6
section: "5. Desarrollo de juegos 2D y 3D"
author: Ismael Velasco
date: 2026-10-25
weight: 5
---
# Práctica UD6 - Mi primer juego: recoge y gana

| Dato | Valor |
|---|---|
| Módulo | Programación de Dispositivos Móviles (0489) |
| Resultado de aprendizaje | RA5 — Desarrolla juegos 2D y 3D sencillos utilizando motores de juegos |
| Criterios trabajados | a, b, c, d, e, f, g, h, i, j |
| Sesiones | 2 sesiones (2 h + 2 h) |
| Motor | Godot 4.7.2 |
| Entrega | repositorio + memoria + `.apk` |

## 1. Qué hay que hacer

Cada alumno parte del proyecto **IsmaelJuego** y lo convierte en **su juego**: mismo esqueleto, tema distinto y al menos **una mecánica nueva** que no esté en el ejemplo.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">No vale entregar el ejemplo sin cambios: la mecánica nueva es obligatoria y es lo que más puntúa.</span>

### Mecánicas nuevas posibles (elige una)

| Mecánica | Qué añade | Dificultad |
|---|---|---|
| Temporizador | cuenta atrás de 30 s; si llega a 0, se pierde | ⭐ |
| Enemigo que persigue | un `Area2D` que quita puntos al tocarte | ⭐⭐ |
| Vidas | 3 vidas, cada toque con enemigo resta una | ⭐⭐ |
| Niveles | al recoger todo, carga una escena con más monedas | ⭐⭐⭐ |
| Salto | el jugador salta con gravedad propia | ⭐⭐⭐ |

## 2. Requisitos mínimos

| Nº | Requisito | Criterio |
|---|---|---|
| 1 | El juego tiene objetivo, reglas, estados y final escritos | a |
| 2 | Cada objeto del juego es una escena instanciable | b |
| 3 | Al menos dos escenas: juego y menú (o juego y victoria) | c |
| 4 | Un objeto con material propio (color, metallic, roughness) | d |
| 5 | Al menos un cuerpo con física real (caída, rebote o empuje) | e |
| 6 | Sonido en al menos un evento | f |
| 7 | Cámara y luz configuradas | g |
| 8 | `.apk` exportado e instalado en un móvil o emulador | h |
| 9 | Tabla de pruebas con resultados reales + medidas de FPS | i |
| 10 | Memoria con las cinco fases | j |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Los diez requisitos son los diez criterios del RA5. Un requisito sin entregar es un criterio sin evaluar.</span>

## 3. Entrega

```text
apellido_nombre_ud6/
├── proyecto/            ← el proyecto Godot completo (project.godot incluido)
├── capturas/            ← 4 capturas: menú, jugando, victoria, apk en el móvil
├── memoria.md           ← las cinco fases
├── pruebas.md           ← tabla de pruebas con resultado real
└── juego.apk            ← el ejecutable de Android
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El `.apk` se entrega junto al proyecto. Un `.apk` sin proyecto no se puede revisar; un proyecto sin `.apk` no cumple el CE h.</span>

## 4. Rúbrica (sobre 10)

| Bloque | Peso | Qué se valora |
|---|---|---|
| Lógica y diseño (a) | 1,0 | objetivo, reglas, estados y final coherentes |
| Objetos y escenas (b, c) | 1,5 | escenas instanciables, árbol limpio |
| Materiales (d) | 0,75 | material propio con parámetros justificados |
| Físicas (e) | 1,25 | cuerpo con física real, no simulada a mano |
| Sonido (f) | 0,75 | sonido en el evento correcto |
| Cámara e iluminación (g) | 1,0 | cámara actual y luz con sombras |
| Despliegue móvil (h) | 1,5 | `.apk` instalado y funcionando |
| Pruebas y optimización (i) | 1,0 | tabla rellena + medidas |
| Documentación (j) | 0,75 | memoria con las cinco fases |
| Mecánica nueva | 0,5 | originalidad y que funcione |
| **Total** | **10,0** | |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un juego que se cae al arrancar en el móvil no puntúa en el bloque h aunque en el ordenador funcione: se prueba en el dispositivo antes de entregar.</span>

## 5. Reparto de las dos sesiones

| Sesión | Qué se hace | Qué se entrega al final |
|---|---|---|
| 1ª (2 h) | lógica, escenas, objetos, movimiento y colisiones | proyecto jugable en el ordenador |
| 2ª (2 h) | sonido, cámara/luz, pruebas, exportación y memoria | `.apk`, pruebas y memoria |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Al final de la primera sesión el juego tiene que poder jugarse entero en el ordenador. Si no, la segunda se va en arreglar y no en exportar.</span>

## 6. Autoevaluación antes de entregar

- [ ] ¿Está escrito el objetivo del juego en una línea?
- [ ] ¿Los estados son un `enum` y no tres booleanos?
- [ ] ¿Cada moneda es una instancia de la misma escena?
- [ ] ¿El sonido suena al recoger y no al arrancar?
- [ ] ¿La luz proyecta sombras?
- [ ] ¿El `.apk` se ha instalado y probado en el móvil?
- [ ] ¿La tabla de pruebas tiene una fila que falla y su plan de mejora?
- [ ] ¿La memoria está escrita durante el desarrollo?

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La tabla de pruebas con TODO en verde suele indicar que no se han probado los límites: salir de pantalla, recoger la última moneda, tocar el enemigo.</span>

## Bibliografía oficial

- Godot Engine — *Your first 2D game*: <https://docs.godotengine.org/en/stable/getting_started/first_2d_game/index.html>
- Godot Engine — *Exporting for Android*: <https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_android.html>
- Godot Engine — *Input examples*: <https://docs.godotengine.org/en/stable/tutorials/inputs/input_examples.html>
- Boletín Oficial del Estado — *RD 405/2023*: <https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221>
