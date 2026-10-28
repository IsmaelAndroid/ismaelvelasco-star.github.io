---
title: "UD 6 - Usabilidad y accesibilidad"
description: "Índice de la unidad 6: los estándares de usabilidad y accesibilidad, el contraste calculado con la fórmula de las WCAG, las áreas de toque de 48 dp, el escalado de fuente al 200 % y las pruebas con expertos y usuarios (RA4 completo)."
authors:
    - Ismael Velasco
date: 2026-10-27
icon: "material/human-wheelchair"
permalink: /di/unidad6/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - usabilidad
    - accesibilidad
    - WCAG
---

# UD 6 — Usabilidad y accesibilidad

Una interfaz no es usable porque quede bonita: es usable cuando se **mide**. En esta unidad se audita el
kit que ya está construido —`BadgeEstado`, `SelectorMes`, `Tecla` y las pantallas del `MainActivity`— con
los criterios de las **WCAG 2.2** y de la **ISO 9241-11**, y la auditoría **la hace la propia app**: se
programa el cálculo del contraste, el detector de áreas de toque y las pruebas que lo demuestran. El
resultado son dos defectos reales encontrados en nuestro propio kit: el badge de aviso se queda en
**3,37:1** y la tecla del piano mide **44 dp** de ancho cuando el mínimo son 48.

## Contenido

- [Teoría 6.1 — Usabilidad y accesibilidad](teoria/DI-U6.1.-UsabilidadYAccesibilidad.md)

## Presentación

- [DI-U6.1 - Usabilidad y accesibilidad](https://revilofe.github.io/slides/section6-di/DI-U6.1.-UsabilidadYAccesibilidad.html)

## Qué aprenderás

- Identificar los **estándares** que se citan en cualquier auditoría: ISO/IEC 9241 (partes 10 a 17),
  ISO/IEC 9126-1 y su heredera 25010, ISO 14915, ISO 13407 → 9241-210, ISO/TR 16982, **WCAG 2.2**,
  **EN 301 549** y la definición oficial de usabilidad de la **UNE-EN ISO 9241-11:2018**.
- **Calcular** el contraste con la fórmula de las WCAG (luz lineal, luminancia relativa y ratio) y
  entender por qué 21:1 y 1:1 son los valores que delatan una implementación mal hecha.
- La trampa del **color con alfa**: hay que componerlo sobre el fondo real antes de medirlo, o la
  auditoría da 1:1 y miente.
- Medir **áreas de toque**: 48 dp en Android, 24 px el mínimo legal (WCAG 2.5.8) y 44 px el AAA (2.5.5).
  La zona que responde al dedo, no el dibujo.
- Probar el **escalado de fuente al 200 %** (WCAG 1.4.4): texto en `sp`, reflow sin cortes y sin scroll
  horizontal.
- Evaluar con **expertos y con usuarios**: las diez heurísticas de Nielsen, la escala de gravedad 0-4,
  el cuestionario **SUS** y por qué con cinco usuarios se encuentra el 85 % de los problemas.
- Escribir **pruebas automáticas** de accesibilidad: los números van en `src/androidTest` y no caducan.

## Proyecto de la unidad

El mismo `IsmaelUIKit` de las unidades anteriores, ampliado con la auditoría:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | Todo lo de las UD4 y UD5 + `Contraste` (fórmula WCAG y composición de alfa), `ParContraste`, `ControlToque`, `PanelContraste` y `PanelToques` |
| `:app` | `com.example.uikit.demo` | Siete escenas: catálogo, figuras, recarga de abono, puntero, piano, persistencia y **auditoría del kit** |
| pruebas | `com.example.uikit` (androidTest) | Las 11 de la UD5 + `UsabilidadTest` con 5 pruebas nuevas — **16 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto, compilado y ejecutado en el emulador: las
tres capturas son del emulador, el volcado de `uiautomator` está pegado tal cual salió y la salida de
pruebas (`OK (16 tests)` · `Time: 20.029`) es la que devolvió `am instrument`.

## Resultado de aprendizaje

**RA4. Diseña interfaces gráficas identificando y aplicando criterios de usabilidad y accesibilidad** —
la unidad lo trabaja **completo**: los nueve criterios de evaluación, de la `a` a la `i`.

## Relación con otros módulos

Los criterios de accesibilidad no son de este módulo: son de **cualquier** interfaz. En **PMDM**
(módulo 0489) se aplican igual a las pantallas de una app móvil, y en la **UD8** de este mismo módulo la
auditoría se convierte en el **informe** del RA5: lo que aquí se mide con código, allí se documenta con
capturas, tablas y conclusiones.
