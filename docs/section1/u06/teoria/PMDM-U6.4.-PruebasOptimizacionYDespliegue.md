---
title: "Tema 6.4 - Pruebas, optimización y despliegue en móvil"
summary: "Cómo se prueba un juego de verdad, qué se mide para optimizarlo y cómo se exporta a Android, con los requisitos reales de Godot 4.7.2."
series: "Programación de Dispositivos Móviles"
series_order: 6
section: "5. Desarrollo de juegos 2D y 3D"
author: Ismael Velasco
date: 2026-10-25
weight: 4
---
# Tema 6.4 - Pruebas, optimización y despliegue en móvil

Un juego que solo funciona en tu ordenador no está terminado. Este tema cierra el RA5 con los tres criterios que faltan: pruebas (CE i), despliegue en móvil (CE h) y documentación (CE j).

<div style="background-color:#a5d6a7; border-radius:8px; padding:10px 14px; margin:14px 0;">
<strong>🟢 Leyenda de repaso obligatorio:</strong> los textos con <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">este fondo verde</span> son imprescindibles y entran en todas las evaluaciones del módulo.</div>

## 1. Probar no es jugar

Probar es repetir una acción con un resultado esperado escrito antes. Se hace con una tabla y se apunta el resultado:

| Nº | Acción | Resultado esperado | Resultado real | ¿OK? |
|---|---|---|---|---|
| 1 | Arrancar el juego | aparece el escenario y `monedas: 0 / 3` | `monedas: 0 / 3` | ✅ |
| 2 | Pulsar flecha derecha 1 s | el jugador avanza a la derecha | avanza ~320 px | ✅ |
| 3 | Tocar la moneda 1 | desaparece y marca `1 / 3` | `1 / 3` y pitido | ✅ |
| 4 | Recoger las tres | mensaje de victoria | `¡Has ganado! 3 / 3` | ✅ |
| 5 | Salir de la pantalla | el jugador se sale del marco | se sale | ❌ pendiente |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Una prueba se escribe con resultado esperado ANTES de ejecutarla. Si lo escribes después, estás justificando lo que ha pasado, no probando.</span>

La columna «resultado real» es la que convierte la tabla en evidencia. Y la última fila es la más útil: **una prueba que falla vale más que diez que pasan**, porque es la que te dice qué mejorar.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La prueba que falla es la que aporta información. Un informe con todo en verde suele significar que las pruebas eran demasiado fáciles.</span>

## 2. Qué se mide al optimizar (CE i)

Optimizar sin medir es adivinar. En Godot las tres medidas que importan en móvil:

| Medida | Dónde se ve | Valor sano en móvil | Qué la empeora |
|---|---|---|---|
| FPS | monitor de depuración (`Depurar → Monitores`) | 60 | demasiados nodos, física compleja |
| Tiempo de fotograma | mismo monitor | < 16,6 ms | scripts pesados en `_process` |
| Memoria | mismo monitor | estable, sin subir | texturas enormes, nodos que se crean y no se liberan |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">60 FPS = 16,6 ms por fotograma. Si el tiempo de fotograma pasa de 16,6 ms, hay tirones, aunque el juego «vaya».</span>

Las cuatro optimizaciones que de verdad se notan en un juego pequeño:

1. **No crear nodos en `_process`**: se crean al empezar y se reutilizan.
2. **`queue_free()` cuando algo desaparece**: si no, la memoria sube sin parar.
3. **Texturas ajustadas**: una moneda de 36 px no necesita una imagen de 2048 px.
4. **Desactivar lo que no se ve**: `visible = false` en lo que está fuera de pantalla.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Crear y liberar nodos 60 veces por segundo es la causa número uno de tirones en juegos de principiante.</span>

## 3. Exportar a Android (CE h)

Godot exporta a Android con un `.apk` que se instala en el móvil. Los pasos reales:

| Paso | Qué se hace | Dónde |
|---|---|---|
| 1 | Instalar **Android Studio** y el SDK | <https://developer.android.com/studio> |
| 2 | Instalar las **plantillas de exportación** de la versión exacta de Godot | Editor → `Editor → Manage Export Templates` |
| 3 | Configurar el SDK en Godot | `Editor → Editor Settings → Export → Android` |
| 4 | Crear el preset **Android** | `Proyecto → Exportar → Añadir → Android` |
| 5 | Rellenar el nombre del paquete | `com.iesrafaelalberti.isameljuego` |
| 6 | Exportar el `.apk` | botón «Exportar proyecto» |
| 7 | Instalar en el móvil | `adb install juego.apk` |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Sin las plantillas de exportación de la MISMA versión de Godot no se puede exportar: el botón da error aunque el SDK esté bien.</span>

La firma de depuración la genera Godot sola. Para publicar en Google Play hace falta además un **keystore** propio:

```bash
# genera un keystore de publicación (válido 25 años)
keytool -genkey -v -keystore mi-juego.keystore -alias mijuego \
        -keyalg RSA -keysize 2048 -validity 9125
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El keystore de publicación no se pierde nunca: sin él no se puede actualizar la app en Google Play. Se guarda fuera del repositorio.</span>

En el móvil, además, hay que **adaptar la entrada**: no hay flechas. Se usan botones en pantalla o el acelerómetro:

```gdscript
# Botones táctiles: cada botón emite pressed/released
func _on_boton_derecha_button_down() -> void:
	direccion.x = 1.0        # equivale a mantener pulsada la flecha derecha
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">En móvil se sustituye `Input.get_vector` por botones táctiles o por `Input.get_gravity()` si se usa el acelerómetro.</span>

## 4. Ajustes propios de móvil

| Ajuste | Por qué | Dónde |
|---|---|---|
| `display/window/handheld/orientation` | fijar horizontal o vertical | Ajustes del proyecto |
| Escalado `canvas_items` | que el juego se adapte a cualquier pantalla | Ajustes del proyecto |
| Renderizador `gl_compatibility` | funciona en móviles modestos | Ajustes del proyecto |
| Pantalla completa | quitar las barras del sistema | Ajustes del proyecto |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`gl_compatibility` es el renderizador recomendado para clase: funciona en cualquier móvil y en el emulador. `forward_plus` pide Vulkan y deja fuera a móviles antiguos.</span>

## 5. Documentar el desarrollo (CE j)

La memoria del juego es una tabla, no un ensayo:

| Fase | Qué se entrega |
|---|---|
| Diseño | objetivo, reglas, estados y objetos |
| Desarrollo | árbol de escena, scripts y capturas |
| Pruebas | tabla de pruebas con resultado real |
| Optimización | medidas de FPS y memoria antes/después |
| Despliegue | pasos del `.apk` y captura en el móvil |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La memoria se escribe mientras se desarrolla. Reconstruirla al final siempre miente en algo.</span>

## 6. Lista de comprobación final

Antes de dar el juego por terminado:

- [ ] El juego arranca sin errores en la consola.
- [ ] Se puede jugar de principio a fin.
- [ ] El marcador cuenta bien.
- [ ] Hay sonido en al menos un evento.
- [ ] La tabla de pruebas está rellena con resultados reales.
- [ ] Se ha medido FPS y memoria.
- [ ] Se ha exportado el `.apk` y se ha instalado en un móvil.
- [ ] La memoria tiene las cinco fases.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un juego sin `.apk` instalado en un móvil no cumple el CE h, por bien que funcione en el ordenador.</span>

## Bibliografía oficial

- Godot Engine — *Exporting for Android*: <https://docs.godotengine.org/en/stable/tutorials/export/exporting_for_android.html>
- Godot Engine — *Export templates*: <https://docs.godotengine.org/en/stable/tutorials/export/exporting_projects.html>
- Godot Engine — *Optimization*: <https://docs.godotengine.org/en/stable/tutorials/performance/index.html>
- Godot Engine — *Handling quit requests*: <https://docs.godotengine.org/en/stable/tutorials/inputs/handling_quit_requests.html>
- Android Developers — *Android Studio*: <https://developer.android.com/studio>
- Boletín Oficial del Estado — *RD 405/2023*: <https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221>
