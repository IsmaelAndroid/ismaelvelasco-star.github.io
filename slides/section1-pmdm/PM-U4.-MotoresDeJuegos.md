# PM-U4 - Motores de juegos

Note: Unidad 4 en un deck. Se pasa de "una app con pantallas" a "un juego con bucle". El proyecto de referencia es IsmaelArcade, ejecutado en Godot 4.7.2 durante las clases.

---

## Índice de la unidad

Note: Los cuatro temas van de lo general a lo concreto: arquitectura, piezas del motor, comparativa de motores y taller de arranque.

- **4.1** Arquitectura de un juego 2D y 3D
- **4.2** Componentes de un motor de juegos
- **4.3** Entornos de desarrollo y comparativa de motores
- **4.4** Taller: primer proyecto, ejecutar y depurar

---

## 4.1 · Un juego son cuatro piezas

Note: Cuatro piezas y ninguna sobra. Si falta el bucle no hay animación; si falta la escena no hay objetos; sin entrada no se juega; sin render no se ve. Preguntar en clase por cada una con un ejemplo.

| Pieza | Qué aporta | Dónde se ve |
|---|---|---|
| **Bucle** | actualiza y dibuja 60 veces por segundo | `_process` / `_physics_process` |
| **Escena** | árbol de nodos con los objetos | fichero `.tscn` |
| **Entrada** | teclado, táctil, mando | `Input` |
| **Render** | dibuja el resultado | cámara + mallas/sprites |

- Un juego **no** es un `while (true)`: el motor llama a tu código en cada fotograma

---

## 4.1 · El bucle de juego

Note: El bucle tiene dos ritmos: dibujo y física. La física va a paso fijo (60 Hz) para que las colisiones sean estables. Mezclarlos es el error que hace que el jugador atraviese paredes.

```gdscript
func _physics_process(delta: float) -> void:
    # paso fijo: mover cuerpos y comprobar colisiones
    velocity = direccion * 320.0      # píxeles por segundo
    move_and_slide()                  # aplica y resuelve choques

func _process(delta: float) -> void:
    # cada fotograma: cosas visuales (HUD, animaciones)
    etiqueta.text = "FPS: %d" % Engine.get_frames_per_second()
```

- `_physics_process` = **paso fijo** (60 Hz): física y colisiones
- `_process` = **cada fotograma**: lo visual
- `delta` = segundos desde el fotograma anterior

---

## 4.1 · La escena es un árbol de nodos

Note: Godot construye por composición: cada nodo hace una cosa y el árbol las junta. Un jugador es un CharacterBody2D con un dibujo y una forma de colisión. En 3D es igual, cambiando 2D por 3D.

```text
Juego2D (Node2D)              ← la raíz de la escena
├── Jugador (CharacterBody2D)  ← el cuerpo que se mueve
│   ├── Cuerpo (Polygon2D)     ← lo que se ve
│   └── Colision (CollisionShape2D) ← lo que choca
├── HUD (CanvasLayer)          ← marcador fijo en pantalla
└── Sonido (AudioStreamPlayer) ← efectos
```

- **Lo que se ve** y **lo que choca** son nodos distintos
- Cambiar uno sin el otro = el choque no coincide con la imagen

---

## 4.1 · Qué cambia entre 2D y 3D

Note: La arquitectura es la misma; cambian los nodos y aparece la cámara en profundidad, la luz y los materiales. Si se entiende el 2D, el 3D es un cambio de nombres.

| Concepto | 2D | 3D |
|---|---|---|
| Cuerpo del jugador | `CharacterBody2D` | `CharacterBody3D` |
| Cuerpo físico | `RigidBody2D` | `RigidBody3D` |
| Lo que se ve | `Sprite2D` / `Polygon2D` | `MeshInstance3D` |
| Cámara | `Camera2D` | `Camera3D` |
| Luz | opcional (`PointLight2D`) | `DirectionalLight3D`, `OmniLight3D` |

- En 3D aparece lo que en 2D no existe: **materiales, luces y sombras**

---

## 4.2 · Un motor no es «lo que dibuja»

Note: Un motor es un conjunto de subsistemas ya resueltos. El alumno escribe la lógica; el motor pone el render, la física y la entrada. Ese reparto es lo que justifica usar un motor en vez de programar desde cero.

| Subsistema | Qué resuelve |
|---|---|
| Render | dibujar a 60 FPS |
| Física | choques, gravedad, cuerpos |
| Entrada | teclado, táctil, mando |
| Audio | reproducir sonidos y música |
| Recursos | cargar texturas, mallas, escenas |
| Scripting | ejecutar la lógica (GDScript) |

- El motor **no** programa tu juego: te ahorra el render y la física

---

## 4.2 · Escena y recurso no son lo mismo

Note: La escena es un árbol que se instancia; el recurso es un dato que se comparte. Una moneda es escena; su textura es recurso. Confundirlos hace que al cambiar un color cambien las 20 monedas sin querer.

| | Escena | Recurso |
|---|---|---|
| Qué es | árbol de nodos | dato reutilizable |
| Ejemplo | `moneda.tscn` | `moneda.png`, material |
| Se instancia | sí (varias copias) | no (se comparte) |
| Si lo cambio | cambia esa instancia | cambian **todas** las que lo usan |

- Regla: si algo se repite y se comporta igual → **escena instanciable**

---

## 4.2 · Señales: avisar sin estar mirando

Note: La señal es el mecanismo de aviso del motor. La moneda avisa de que la han tocado; el juego escucha y suma. Nadie consulta a nadie cada fotograma: eso es lo que evita el código espagueti.

```gdscript
# la moneda avisa cuando un cuerpo entra en su área
moneda.body_entered.connect(_al_recoger.bind(moneda))

func _al_recoger(cuerpo: Node2D, moneda: Area2D) -> void:
    if cuerpo != jugador:      # solo cuenta el jugador
        return
    puntos += 1                # suma
    moneda.queue_free()        # desaparece
```

- `connect` = **suscribirse** a un aviso
- `bind(x)` añade un argumento extra a la función

---

## 4.3 · Cómo se elige un motor (cinco criterios)

Note: No hay motor mejor: hay motor para un proyecto. Los cinco criterios son los que se preguntan en el examen. Godot gana en clase por licencia, tamaño y arranque rápido.

| Criterio | Pregunta que responde |
|---|---|
| **Licencia y coste** | ¿puedo publicar sin pagar? |
| **Plataformas** | ¿sale en Android y en PC? |
| **Lenguaje** | ¿qué tengo que aprender? |
| **Curva y comunidad** | ¿hay documentación y ejemplos? |
| **Peso y requisitos** | ¿arranca en los equipos del aula? |

- En este módulo: **Godot 4.7.2** por licencia MIT, peso ligero y export a Android

---

## 4.3 · El caso de Android

Note: No todos los motores exportan igual a móvil. Godot exporta APK, pero necesita las plantillas de la misma versión. Unity y Unreal pesan mucho más y piden cuentas y licencias.

- Godot exporta **APK** con plantillas de exportación de la **misma versión**
- El renderizador `gl_compatibility` funciona en móviles modestos
- En móvil no hay flechas: **botones táctiles** o sensores
- Un juego que solo corre en el PC **no cumple** el criterio de despliegue

---

## 4.4 · Taller: los pasos que no se saltan

Note: Ocho pasos en orden. El error típico es saltarse el paso 2 (importar) y luego preguntar por qué no arranca. Se hace en clase paso a paso, sin prisa.

1. Crear el proyecto con el renderizador correcto
2. Abrir el proyecto una vez para que **importe** los recursos
3. Montar la escena principal (raíz + jugador + HUD)
4. Añadir la entrada y mover el jugador
5. Añadir un objeto con colisión (moneda)
6. Conectar la señal y sumar puntos
7. Ejecutar y **leer la consola** si falla
8. Medir FPS antes de decir "va bien"

- Sin el paso 2, el `AudioStreamPlayer` da error de carga

---

## 4.4 · Leer un error de verdad

Note: Un error se lee entero, buscando el primero: los de debajo suelen ser consecuencias. Aquí la causa es que sonido.wav no se puede cargar; por eso la escena falla en su línea 77. El número de línea es la pista más valiosa. Se practica con errores provocados a propósito.

```text
ERROR: No loader found for resource: res://sonido.wav (expected type: AudioStream)
   at: _load (core/io/resource_loader.cpp:332)
ERROR: res://juego2d.tscn:77 - Parse Error: [ext_resource] referenced non-existent resource
```

- El **primer** error suele ser la causa (sonido.wav no carga); los siguientes, sus consecuencias
- `res://juego2d.tscn:77` = fichero y **línea** exacta
- "non-existent resource" = el recurso no está importado o la ruta está mal

---

## 4.4 · Medir en vez de opinar

Note: "Va fluido" no es una medida. Se mira el monitor de depuración: FPS y tiempo de fotograma. Si el fotograma pasa de 16,6 ms hay tirones aunque el contador diga 60.

- 60 FPS = **16,6 ms** por fotograma
- Monitor de depuración: FPS, tiempo de fotograma y memoria
- Si la memoria sube sin parar: nodos que se crean y no se liberan
- Se mide **antes y después** de cada cambio

---

## Resumen exprés de la unidad

Note: Cinco frases para el examen. Repetirlas y pedirlas al día siguiente.

- Un juego = **bucle + escena + entrada + render**
- La física va en `_physics_process`; lo visual, en `_process`
- La escena es un **árbol de nodos**; lo que se ve y lo que choca son nodos distintos
- Las **señales** avisan sin consultar cada fotograma
- Motor elegido con **cinco criterios**, no por moda

---

## Qué entra en la práctica

Note: Avisar el primer día: es lo que se corrige.

- Proyecto que **arranca** y se mueve con el teclado
- Un objeto con **colisión y señal** que cambia el marcador
- HUD en `CanvasLayer` que no se mueve con el mundo
- **Medida de FPS** con el monitor de depuración
- Captura de pantalla del juego funcionando
