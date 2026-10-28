# PM-U6 - Juegos 2D y 3D

Note: Última unidad del módulo. Aquí se junta todo: datos, multimedia y motor. El proyecto es IsmaelJuego, en Godot 4.7.2, con una escena 2D jugable y una escena 3D con física real.

---

## Índice de la unidad

Note: De la lógica al móvil. Primero se piensa el juego, después se construye en 2D, luego en 3D y al final se prueba y se exporta.

- **6.1** Del prototipo al juego: lógica, estados y objetos
- **6.2** Juego 2D: movimiento, colisiones y puntuación
- **6.3** Juego 3D: materiales, físicas, cámara e iluminación
- **6.4** Pruebas, optimización y despliegue en móvil

---

## 6.1 · Qué diferencia un prototipo de un juego

Note: Un prototipo demuestra que el motor arranca. Un juego tiene objetivo, reglas, estado y final. Sin esas cuatro piezas es una demo técnica, y eso es exactamente lo que diferencia un 5 de un 9 en la práctica.

| Pieza | Pregunta |
|---|---|
| **Objetivo** | ¿qué hay que conseguir? |
| **Reglas** | ¿qué se puede y qué no? |
| **Estado** | ¿en qué situación está ahora? |
| **Final** | ¿cuándo se acaba? |

- La lógica se escribe **antes** que el código, y no menciona el motor

---

## 6.1 · Máquina de estados: no es un `if` gigante

Note: Tres estados con dos booleanos permiten combinaciones imposibles (jugando y ganado a la vez). Con un enum no. Se pregunta en el examen.

```gdscript
enum Estado { MENU, JUGANDO, GANADO }   # 0, 1, 2

var estado: int = Estado.MENU           # se empieza en el menú

func _cambiar_estado(nuevo: int) -> void:
    estado = nuevo
    match estado:                       # el switch de GDScript
        Estado.MENU:    $HUD/Mensaje.text = "Pulsa JUGAR"
        Estado.JUGANDO: $HUD/Mensaje.text = ""
        Estado.GANADO:  $HUD/Mensaje.text = "¡Has ganado!"
```

- Un `enum`, **no** dos booleanos
- Cada estado nuevo se añade sin reescribir la lógica

---

## 6.2 · El juego 2D: cuerpos y áreas

Note: Elegir mal el tipo de cuerpo es el error más caro. El jugador es CharacterBody2D porque lo mueve el usuario. La moneda es Area2D porque no frena, solo avisa.

| Nodo | Quién lo mueve | Uso |
|---|---|---|
| `CharacterBody2D` | tu código | jugador, enemigos |
| `RigidBody2D` | la física | cajas que caen |
| `StaticBody2D` | nadie | paredes, suelo |
| `Area2D` | nadie | detectar contactos |

- Jugador `CharacterBody2D` + moneda `Area2D` → señal **`body_entered`**
- `area_entered` **no** se dispara en ese caso

---

## 6.2 · Movimiento en tres líneas

Note: La diagonal es el bug clásico: si se suman las teclas a mano, la diagonal corre un 41% más. get_vector lo normaliza. Y se multiplica por velocidad, no por delta: move_and_slide ya aplica el tiempo.

```gdscript
func _physics_process(_delta: float) -> void:
    # get_vector normaliza: la diagonal no corre más que la recta
    var direccion := Input.get_vector("ui_left", "ui_right", "ui_up", "ui_down")
    jugador.velocity = direccion * 320.0   # píxeles por SEGUNDO
    jugador.move_and_slide()               # mueve y resuelve choques
```

- `get_vector` **normaliza** la diagonal
- `velocity` va en píxeles por segundo: no multiplicar por `delta` a mano

---

## 6.2 · Recoger, sumar y sonar

Note: La señal se conecta al arrancar y trae la moneda con bind. La comprobación de quién entra evita que cualquier cuerpo sume puntos. El sonido va en el evento, no en bucle.

```gdscript
func _al_recoger(cuerpo: Node2D, moneda: Area2D) -> void:
    if cuerpo != jugador:      # solo cuenta el jugador
        return
    puntos += 1
    sonido.play()              # sonido en el evento (CE f)
    moneda.queue_free()        # desaparece al final del fotograma
    etiqueta.text = "monedas: %d / %d" % [puntos, total]
```

- `body_entered` pasa el cuerpo; `bind(moneda)` añade la moneda
- `queue_free()` libera el nodo: no volver a tocarlo después

---

## 6.3 · Materiales: el aspecto de la superficie

Note: El material decide si parece plástico o metal. metallic y roughness solo se notan si hay luz: un metal sin entorno se ve negro. Y el material no basta: hace falta malla.

| Propiedad | Valores | Efecto |
|---|---|---|
| `albedo_color` | color | color base |
| `metallic` | 0 – 1 | 0 plástico, 1 metal |
| `roughness` | 0 – 1 | 0 espejo, 1 mate |
| `emission_enabled` | sí/no | el objeto emite luz |

- Un `MeshInstance3D` **sin malla** no se ve, aunque tenga material
- El material va a una superficie: sin malla no hay superficie

---

## 6.3 · Físicas: cae de 2,46 m a 0,69 m

Note: Datos reales medidos en la escena del aula: la caja empieza a 3,2 m, a los 0,4 s va por 2,46 m y a los 2,5 s descansa a 0,69 m. La altura la mide el motor, no una variable nuestra.

| Propiedad | Qué controla |
|---|---|
| `mass` | respuesta a impulsos |
| `gravity_scale` | multiplicador de la gravedad |
| `bounce` | rebote (en `PhysicsMaterial`) |
| `friction` | rozamiento (en `PhysicsMaterial`) |

- `bounce` y `friction` van en **`PhysicsMaterial`**, no en el material visual
- Un `RigidBody3D` se mueve con **impulsos**, no cambiando `position`

---

## 6.3 · Cámara y luz

Note: La cámara es el ojo del jugador y la luz decide si se ve algo. La direccional solo cuenta la rotación; moverla no cambia nada. Sin luz, la malla se ve negra.

- `Camera3D`: `fov` 75°, `near` 0,05, `far` 4000, **`current`** activada
- En un `Transform3D`, los **tres últimos** números son la posición
- `DirectionalLight3D` = sol: solo importa la **rotación**
- `OmniLight3D` = bombilla: importa la posición y el alcance
- `shadow_enabled` para que los objetos no floten

---

## 6.4 · Probar no es jugar

Note: Una prueba se escribe con resultado esperado ANTES de ejecutarla. La fila que falla es la más valiosa: dice qué mejorar. Un informe con todo en verde suele significar pruebas demasiado fáciles.

| Nº | Acción | Esperado | Real |
|---|---|---|---|
| 1 | Arrancar | `monedas: 0 / 3` | ✅ |
| 2 | Flecha derecha 1 s | avanza ~320 px | ✅ |
| 3 | Tocar moneda | `1 / 3` y pitido | ✅ |
| 4 | Recoger las tres | `¡Has ganado! 3 / 3` | ✅ |
| 5 | Salir de pantalla | el jugador se sale | ❌ pendiente |

- Resultado esperado **antes**, no después
- La prueba que falla es la que informa

---

## 6.4 · Optimizar es medir

Note: 60 FPS son 16,6 ms por fotograma. Si se pasa de ahí hay tirones aunque el contador diga 60. Las cuatro optimizaciones que se notan en un juego pequeño.

| Medida | Sano en móvil | Qué la empeora |
|---|---|---|
| FPS | 60 | demasiados nodos |
| Tiempo de fotograma | < 16,6 ms | scripts pesados |
| Memoria | estable | nodos que no se liberan |

- No crear nodos en `_process`; reutilizarlos
- `queue_free()` cuando algo desaparece
- Texturas ajustadas al tamaño real en pantalla

---

## 6.4 · Exportar a Android

Note: Sin plantillas de exportación de la MISMA versión no hay APK, aunque el SDK esté perfecto. Es el error que se come la segunda sesión de la práctica.

1. Android Studio + SDK
2. **Plantillas de exportación** de la misma versión de Godot
3. Configurar el SDK en Godot
4. Preset **Android** y nombre de paquete (`com.iesrafaelalberti....`)
5. Exportar el `.apk`
6. `adb install juego.apk`

- Sin plantillas → el botón de exportar falla
- El keystore de publicación **no se pierde**: sin él no hay actualizaciones
- En móvil no hay flechas: botones táctiles o sensores

---

## Resumen exprés de la unidad

Note: Cinco frases. La primera y la última son las que deciden la nota de la práctica.

- Un juego = **objetivo + reglas + estado + final**
- Estados con **`enum`**, no con booleanos
- `CharacterBody2D` para el jugador, `Area2D` para lo que se recoge
- En 3D: **material + malla + forma de colisión** (las tres)
- Sin **`.apk` instalado** en un móvil no se cumple el RA5

---

## Qué entra en la práctica

Note: Es la rúbrica, resumida. Enseñarla el primer día.

- Juego con **mecánica nueva** propia (no el ejemplo tal cual)
- Escenas instanciables + menú o pantalla de victoria
- Un cuerpo con **física real** y sonido en un evento
- Tabla de pruebas con resultado real y **medidas de FPS**
- `.apk` instalado en móvil o emulador + memoria con las cinco fases
