---
title: "Tema 4.3 - Entornos de desarrollo y comparativa de motores"
summary: "Qué es un entorno de desarrollo de juegos, qué motor elegir según el proyecto y comparativa de Godot, Unity, Unreal, libGDX y Phaser con versiones verificadas."
series: "Programación de Dispositivos Móviles"
series_order: 4
section: "4. Selección y prueba de motores de juegos"
author: Ismael Velasco
date: 2026-10-25
weight: 3
---
# Tema 4.3 - Entornos de desarrollo y comparativa de motores

Este apartado es de análisis, no de teclado: hay que saber **qué hay en el mercado, qué cuesta y para qué sirve cada motor** antes de elegir. Los números de versión están comprobados en la fuente oficial de cada proyecto el 25 de octubre de 2026.

<div style="background-color:#a5d6a7; border-radius:8px; padding:10px 14px; margin:14px 0;">
<strong>🟢 Leyenda de repaso obligatorio:</strong> los textos con <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">este fondo verde</span> son imprescindibles y entran en todas las evaluaciones del módulo.</div>

## 1. Qué es un entorno de desarrollo de juegos

Un entorno de desarrollo de juegos es el programa donde montas el juego. Tiene cuatro partes:

1. **Editor de escenas**: donde colocas los objetos y ves el resultado.
2. **Motor**: el que ejecuta el bucle, la física y el render.
3. **Editor de código**: donde escribes la lógica.
4. **Exportadores**: los que generan el ejecutable para cada plataforma.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El motor es la parte que se ejecuta en el juego; el entorno es el programa con el que lo construyes. Godot, Unity y Unreal incluyen las dos cosas en el mismo instalador.</span>

## 2. Comparativa de motores

| Motor | Versión verificada (25/10/2026) | Licencia | Lenguaje | 2D / 3D | Android | Cuándo elegirlo |
|---|---|---|---|---|---|---|
| **Godot** | 4.7.2 stable | MIT (software libre) | GDScript, C# | 2D y 3D | sí | proyectos de clase, 2D, equipos pequeños, sin cuotas |
| **Unity** | Unity 6 LTS | propietaria, con plan gratuito sujeto a condiciones | C# | 2D y 3D | sí | salida laboral, móvil comercial, muchos assets |
| **Unreal Engine** | Unreal Engine 5 | propietaria, con royalties por encima de un umbral | C++, Blueprints | 3D sobre todo | sí | 3D de alta calidad, consolas y PC |
| **libGDX** | 1.13.1 | Apache 2.0 | Java, Kotlin | 2D y 3D básico | sí | aprender cómo funciona un motor por dentro, Java |
| **Phaser** | 4.2.1 | MIT | JavaScript, TypeScript | 2D | web móvil | juegos web, prototipos rápidos |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Godot es software libre con licencia MIT: no hay cuotas ni regalías, y por eso es el motor del aula. Unity y Unreal son propietarios y sus condiciones económicas se consultan en su web, no de memoria.</span>

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Un motor no se elige por moda: se elige por plataforma objetivo, tipo de juego (2D o 3D), licencia y equipo disponible.</span>

## 3. Cómo se elige un motor (los cinco criterios)

| Criterio | Pregunta que hay que hacerse | Ejemplo |
|---|---|---|
| Plataforma | ¿dónde se va a jugar? | si es web, Phaser; si es móvil, Godot o Unity |
| Dimensión | ¿2D o 3D? | un plataformas 2D no necesita Unreal |
| Licencia | ¿cuánto cuesta publicar? | MIT no cobra; Unity y Unreal tienen condiciones |
| Equipo | ¿cuántos sois y qué sabéis? | GDScript se aprende en una tarde; C++ de Unreal, no |
| Ecosistema | ¿hay assets, tutoriales y foro? | Unity tiene la tienda de assets más grande |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El criterio que más se olvida es el equipo: el mejor motor del mundo no sirve si nadie del grupo sabe usarlo. Se elige lo que el equipo puede terminar.</span>

## 4. El caso concreto de Android

En la unidad 1 de **Desarrollo de Interfaces** viste que Android Studio ofrece una plantilla llamada **Game Activity (C++)**. No es un motor: es una plantilla de proyecto en C++ con OpenGL para dibujar a mano.

| | Plantilla Game Activity (C++) | Motor (Godot, Unity…) |
|---|---|---|
| Te da | una ventana con OpenGL | bucle, física, escena, entrada, audio |
| Escribes tú | el render y el bucle completos | solo la lógica y la escena |
| Para qué sirve | aprender qué hay debajo | terminar un juego |

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La plantilla `Game Activity` no es un motor: es un lienzo en C++. Sirve para entender lo que el motor hace por ti, no para hacer un juego en dos semanas.</span>

## 5. Qué motor usa este módulo y por qué

En el aula trabajamos con **Godot 4.7.2**:

- Es **software libre (MIT)**: se instala sin cuenta, sin licencia y sin conexión.
- Es **portable**: un `.zip` de 82 MB, sin instalador; corre en cualquier equipo del taller.
- Permite ver el **árbol de escenas** y el código sin capas ocultas, que es justo lo que hay que analizar en este resultado de aprendizaje.
- Exporta a **Android**, que es la plataforma del módulo.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Godot no es el motor más usado de la industria, pero sí el que mejor deja ver la arquitectura: para aprender qué es un motor, es el más honesto.</span>

## 6. Lo que tienes que llevarte de este apartado

- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Entorno de desarrollo = editor de escenas + motor + editor de código + exportadores.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Godot 4.7.2 (MIT), Unity 6 LTS, Unreal Engine 5, libGDX 1.13.1 (Apache 2.0) y Phaser 4.2.1 (MIT).</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Se elige por plataforma, dimensión (2D/3D), licencia, equipo y ecosistema.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La plantilla Game Activity (C++) de Android Studio no es un motor.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Las condiciones económicas de Unity y Unreal se consultan en su web oficial, nunca de memoria.</span>

## Bibliografía y documentación oficial

- Godot Engine — web y descargas: [godotengine.org](https://godotengine.org/) · [descargas](https://godotengine.org/download/) · [licencia MIT](https://godotengine.org/license/).
- Godot Engine — documentación: [docs.godotengine.org](https://docs.godotengine.org/en/stable/).
- Unity — versiones y planes: [unity.com/releases](https://unity.com/releases) · [manual](https://docs.unity3d.com/Manual/index.html).
- Unreal Engine — documentación y licencia: [dev.epicgames.com — Unreal Engine](https://dev.epicgames.com/documentation/en-us/unreal-engine) · [unrealengine.com/license](https://www.unrealengine.com/en-US/license).
- libGDX — web, wiki y licencia: [libgdx.com](https://libgdx.com/) · [wiki](https://libgdx.com/wiki/) · [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).
- Phaser — documentación y licencia: [phaser.io](https://phaser.io/) · [docs.phaser.io](https://docs.phaser.io/) · [MIT](https://github.com/phaserjs/phaser/blob/master/license.txt).
- Android — desarrollo de juegos: [developer.android.com/games](https://developer.android.com/games).
- Módulo 0489 — Real Decreto 405/2023 (BOE): [BOE-A-2023-13221](https://www.boe.es/buscar/act.php?id=BOE-A-2023-13221).

## Presentación

- [PM-U4 - Motores de juegos](https://revilofe.github.io/slides/section1-pmdm/PM-U4.-MotoresDeJuegos.html)
