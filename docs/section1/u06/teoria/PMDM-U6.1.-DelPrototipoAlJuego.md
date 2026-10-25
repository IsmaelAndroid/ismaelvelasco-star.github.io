---
title: "Tema 6.1 - Del prototipo al juego: lógica, estados y objetos"
summary: "Cómo se pasa de una escena que se mueve a un juego con reglas: máquina de estados, objetos del juego y reparto de responsabilidades entre escenas."
series: "Programación de Dispositivos Móviles"
series_order: 6
section: "5. Desarrollo de juegos 2D y 3D"
author: Ismael Velasco
date: 2026-10-25
weight: 1
---
# Tema 6.1 - Del prototipo al juego: lógica, estados y objetos

En el tema 4 montamos un proyecto que se movía. Eso era un **prototipo**: demuestra que el motor arranca, pero no es un juego, porque no tiene reglas, ni objetivo, ni final. En este tema convertimos el prototipo en un juego de verdad.

<div style="background-color:#a5d6a7; border-radius:8px; padding:10px 14px; margin:14px 0;">
<strong>🟢 Leyenda de repaso obligatorio:</strong> los textos con <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">este fondo verde</span> son imprescindibles y entran en todas las evaluaciones del módulo.</div>

## 1. Qué diferencia un prototipo de un juego

Un juego necesita cuatro cosas que un prototipo no tiene:

| Pieza | Pregunta que responde | Ejemplo en nuestro juego |
|---|---|---|
| Objetivo | ¿qué tengo que conseguir? | recoger las 3 monedas |
| Reglas | ¿qué puedo y qué no puedo hacer? | solo cuenta si lo toca el jugador |
| Estado | ¿en qué situación está el juego ahora? | menú, jugando, terminado |
| Final | ¿cuándo se acaba? | cuando quedan 0 monedas |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un juego = objetivo + reglas + estado + final. Sin esas cuatro piezas es una demo técnica.</span>

El resultado de aprendizaje 5 del módulo (RD 405/2023) pide literalmente «desarrollar juegos 2D y 3D sencillos utilizando motores de juegos», y su primer criterio es <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">establecer la lógica de un nuevo juego</span>. Es decir: antes de tocar el editor, se decide la lógica.

## 2. La lógica se escribe antes que el código

La lógica de un juego se puede contar en una lista numerada, sin nombrar ningún motor:

1. El jugador aparece en la izquierda de la pantalla.
2. Hay tres monedas colocadas en el escenario.
3. El jugador se mueve con las flechas del teclado.
4. Si el jugador toca una moneda, la moneda desaparece y suma un punto.
5. Suena un efecto cada vez que se recoge una moneda.
6. Cuando las tres monedas desaparecen, el juego termina.

Esa lista es el **diseño de la lógica** (CE a). Si mañana el juego se hace en Unity en vez de en Godot, la lista no cambia: solo cambia el código que la implementa.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La lógica se escribe en lenguaje normal y no menciona el motor. Si tu lista menciona nodos o funciones, todavía no es lógica: es implementación.</span>

## 3. Máquina de estados: el juego siempre está en un estado

Un juego no es un `if` gigante. Es una **máquina de estados**: en cada momento está exactamente en uno de sus estados, y cambia cuando ocurre un evento.

| Estado | Qué se ve | Qué lo cambia |
|---|---|---|
| `MENU` | título y botón de jugar | pulsar jugar → `JUGANDO` |
| `JUGANDO` | el escenario y el marcador | recoger la última moneda → `GANADO` |
| `GANADO` | mensaje de victoria | pulsar reiniciar → `JUGANDO` |

En GDScript se implementa con un `enum` y una variable de estado:

```gdscript
# Los estados posibles del juego. enum crea constantes con nombre:
# 0 = MENU, 1 = JUGANDO, 2 = GANADO
enum Estado { MENU, JUGANDO, GANADO }

var estado: int = Estado.MENU   # el juego empieza en el menú


func _cambiar_estado(nuevo: int) -> void:
	estado = nuevo                        # guarda el estado nuevo
	match estado:                         # match es el switch de GDScript
		Estado.MENU:
			$HUD/Mensaje.text = "Pulsa JUGAR"
		Estado.JUGANDO:
			$HUD/Mensaje.text = ""        # sin mensaje mientras se juega
		Estado.GANADO:
			$HUD/Mensaje.text = "¡Has ganado!"
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un estado por variable, no por booleano. Tres estados con dos booleanos (`jugando` y `ganado`) permiten combinaciones imposibles; un `enum` no.</span>

Ventaja real de la máquina de estados: cuando el juego crezca (pausa, derrota, tienda), se añade un estado más y no hay que reescribir la lógica entera.

## 4. Objetos del juego: cada cosa es una escena

El criterio b pide <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">crear los objetos necesarios para el juego y definir sus características</span>. En Godot, un objeto del juego es una **escena** que se puede instanciar muchas veces:

| Objeto | Tipo de nodo | Características que define |
|---|---|---|
| Jugador | `CharacterBody2D` | velocidad 320 px/s, forma cuadrada de 48×48 |
| Moneda | `Area2D` | forma de 32×32, pertenece al grupo `monedas` |
| Marcador | `Label` dentro de un `CanvasLayer` | tamaño de letra 32, texto `monedas: 0 / 3` |
| Sonido | `AudioStreamPlayer` | fichero `sonido.wav` |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Una moneda es una escena instanciable: si el juego necesita 20 monedas, se instancia 20 veces la misma escena, no se copia y pega el nodo.</span>

El grupo (`groups=["monedas"]`) es la pieza que hace que el código no dependa de los nombres:

```gdscript
# get_nodes_in_group devuelve TODAS las monedas que existan en la escena,
# sin importar cómo se llamen ni cuántas haya.
var monedas := get_tree().get_nodes_in_group("monedas")
total = monedas.size()                 # 3 en nuestro juego, pero podría ser 30
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Agrupar (`grupo monedas`) es lo que permite añadir monedas en el editor sin tocar una línea de código.</span>

## 5. Reparto de responsabilidades

El error clásico del principiante es meter todo en un script de 900 líneas. La regla es: **cada script hace una cosa**.

| Script | Responsabilidad única |
|---|---|
| `juego2d.gd` | la lógica: puntuación, estado y fin de partida |
| `jugador.gd` | moverse y responder a la entrada |
| `moneda.gd` | avisar de que la han tocado y desaparecer |

En nuestro juego el jugador se mueve desde el script principal porque es una sola línea de física, pero en cuanto el jugador tenga salto, animación o vida propia, se lleva a su propio script.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Si un script supera las 150 líneas o mezcla entrada, dibujo y puntuación, está pidiendo una escena propia.</span>

## 6. El bucle que hace que todo ocurra

La lógica no se ejecuta «cuando quiere»: el motor llama a tus funciones en cada fotograma.

| Función | Cuándo la llama el motor | Para qué sirve |
|---|---|---|
| `_ready()` | una vez, al entrar en la escena | conectar señales, contar monedas |
| `_process(delta)` | cada fotograma (dibujo) | animaciones, texto del HUD |
| `_physics_process(delta)` | 60 veces por segundo (física) | mover cuerpos, comprobar choques |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Todo lo que mueva un cuerpo o compruebe una colisión va en `_physics_process`, nunca en `_process`: la física corre a paso fijo y `_process` no.</span>

La `delta` es el tiempo en segundos desde el fotograma anterior. Multiplicar por ella hace que la velocidad no dependa de los FPS:

```gdscript
func _physics_process(_delta: float) -> void:
	# get_vector devuelve un vector normalizado (-1..1) según las flechas
	var direccion := Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")
	jugador.velocity = direccion * 320.0   # 320 px/s, no 320 px por fotograma
	jugador.move_and_slide()               # mueve y resuelve las colisiones
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`direccion * 320.0` con `velocity` da píxeles por segundo. Si multiplicas además por `delta` a mano, el jugador irá lentísimo: `move_and_slide()` ya lo hace.</span>

## 7. Ejemplo completo y ejecutado: la lógica del juego

Este es el script real de la escena `juego2d.tscn` del proyecto **IsmaelJuego**, ejecutado en Godot 4.7.2. Implementa las seis reglas de la lista del apartado 2:

```gdscript
extends Node2D
# ============================================================
# UD6 · Juego 2D: movimiento, colisiones, puntuación y sonido
# RA5 · CE a (lógica), b (objetos), c (escenas), e (físicas), f (sonido)
# ============================================================

var puntos: int = 0                                    # monedas recogidas
var total: int = 0                                     # monedas que hay en la escena

@onready var etiqueta: Label = $HUD/Puntos             # texto del marcador
@onready var sonido: AudioStreamPlayer = $Sonido       # efecto al recoger
@onready var jugador: CharacterBody2D = $Jugador       # el personaje


func _ready() -> void:
	# se conecta la señal de cada moneda con la función que suma puntos
	var monedas := get_tree().get_nodes_in_group("monedas")
	total = monedas.size()
	for moneda in monedas:
		moneda.body_entered.connect(_al_recoger.bind(moneda))
	_actualizar_marcador()


func _physics_process(_delta: float) -> void:
	# vector de entrada: flechas o WASD (acciones ui_* que trae Godot)
	var direccion := Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")
	jugador.velocity = direccion * 320.0               # 320 píxeles por segundo
	jugador.move_and_slide()                           # mueve y resuelve colisiones


func _al_recoger(cuerpo: Node2D, moneda: Area2D) -> void:
	if cuerpo != jugador:
		return                                         # solo cuenta si lo toca el jugador
	puntos += 1
	sonido.play()                                      # CE f: sonido en el evento
	moneda.queue_free()                                # la moneda desaparece
	_actualizar_marcador()


func _actualizar_marcador() -> void:
	etiqueta.text = "monedas: %d / %d" % [puntos, total]
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`@onready` retrasa la búsqueda del nodo hasta que la escena existe. Sin `@onready`, `$HUD/Puntos` vale `null` y el juego rompe al arrancar.</span>

Y el final de partida, que es la regla 6, se añade en dos líneas:

```gdscript
func _actualizar_marcador() -> void:
	etiqueta.text = "monedas: %d / %d" % [puntos, total]
	if puntos == total:                    # ya no queda ninguna moneda
		etiqueta.text = "¡Has ganado! %d / %d" % [puntos, total]
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La condición de fin se comprueba al cambiar el marcador, no en `_process`: comprobar lo mismo 60 veces por segundo es trabajo tirado.</span>

## 8. Documentar el diseño (CE j)

El último criterio del RA5 pide <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">documentar las fases de diseño y desarrollo</span>. No es relleno: es lo que permite que otra persona (o tú en junio) entienda el juego sin abrir el editor. La ficha mínima:

| Apartado | Qué se escribe |
|---|---|
| Objetivo | recoger las 3 monedas |
| Reglas | movimiento con flechas, solo cuenta el jugador, la moneda desaparece |
| Estados | `MENU` → `JUGANDO` → `GANADO` |
| Objetos | jugador (CharacterBody2D), moneda (Area2D), HUD, sonido |
| Controles | flechas del teclado |
| Final | marcador 3/3 |

## 9. Errores típicos (y cómo se ven)

| Error | Qué pasa en pantalla | Causa |
|---|---|---|
| Todo en `_process` | el jugador atraviesa las monedas | la física no se resuelve a paso fijo |
| Comparar por nombre de nodo | añades una moneda y no cuenta | falta el grupo `monedas` |
| `queue_free()` y seguir usando el nodo | error «previously freed instance» | se libera y se vuelve a tocar |
| Sumar puntos sin comprobar quién entra | suma al pasar cualquier cuerpo | falta `if cuerpo != jugador` |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">«previously freed instance» significa que has usado un nodo después de `queue_free()`. Se arregla comprobando `is_instance_valid(moneda)` o no volviendo a tocarlo.</span>

## Bibliografía oficial

- Godot Engine — *Nodes and scenes*: <https://docs.godotengine.org/en/stable/tutorials/scripting/nodes_and_scene_instances.html>
- Godot Engine — *Groups*: <https://docs.godotengine.org/en/stable/tutorials/scripting/groups.html>
- Godot Engine — *Idle and Physics processing*: <https://docs.godotengine.org/en/stable/tutorials/scripting/idle_and_physics_processing.html>
- Godot Engine — *GDScript reference*: <https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html>
- Boletín Oficial del Estado — *RD 405/2023, currículo de PMDM*: <https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221>
