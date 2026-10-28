---
title: "UD 11 - Evaluación del funcionamiento: pruebas"
description: "Índice de la unidad 11: estrategia de pruebas, unitarias y de integración, regresión con línea base, volumen y estrés, seguridad, uso de recursos y documentación de resultados (RA8 completo)."
authors:
    - Ismael Velasco
date: 2026-10-28
icon: "material/test-tube"
permalink: /di/unidad11/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - pruebas de software
    - regresión
    - JUnit
    - calidad
---

# UD 11 — Evaluación del funcionamiento: pruebas

Diez unidades construyendo componentes, informes, documentación y una build firmada. Esta es la
unidad que pregunta **si funciona**, y lo pregunta con un plan escrito antes de pulsar nada: ocho
casos, dos de ellos con presupuesto de tiempo y de memoria, una línea base de SHA-256 congelada y
un informe que **escribe la propia aplicación** en el teléfono. Nada de la unidad va de «probar un
poco»: va de poder enseñar un papel que diga qué se probó, con qué, cuánto costó y qué quedó
pendiente.

## Contenido

- [Teoría 11.1 — Evaluación del funcionamiento: pruebas](teoria/DI-U11.1.-EvaluacionDelFuncionamiento.md)

## Presentación

- [DI-U11.1 - Evaluación del funcionamiento: pruebas](https://revilofe.github.io/slides/section6-di/DI-U11.1.-EvaluacionDelFuncionamiento.html)
## Qué aprenderás

- Escribir una **estrategia de pruebas** como datos: tipos, preguntas, herramienta y si el caso es
  manual o automático.
- La diferencia entre **prueba unitaria y de integración**, y qué significa que el BOE hable de
  integración ascendente y descendente.
- **Regresión con golden file**: congelar la salida buena, compararla por huella y
  <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">capturar la referencia de una ejecución real, no inventarla</span>.
- Que una línea base sin una prueba que la muerda es decoración: aquí hay un test que sube un
  céntimo un asiento y exige que la tanda caiga.
- **Volumen y estrés** sobre un banco de datos sintético y determinista, con presupuestos escritos
  (4.000 ms y 200 MB) y con la comprobación de degradación entre la primera y la última pasada.
- **Seguridad barata y útil**: auditar los ficheros que la app escribe y protestar si no hay nada
  que auditar.
- **Uso de recursos** medido con `Runtime`, con la misma medidora para todos los casos para que el
  total se pueda sumar.
- Documentar la tanda: un informe **generado** que incluye lo pendiente, y que entra en el esquema
  de persistencia de la UD9 como un fichero más.
- Leer de una patada la salida de `am instrument` y saber si un rojo es un fallo de código, de
  entorno o de la propia prueba.

## Proyecto de la unidad

El mismo `IsmaelUIKit`, ahora con tanda de pruebas integrada en la propia aplicación:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | Todo lo anterior + `Pruebas` (plan e informe), `BancoPruebas` (banco determinista) y `EjecutaPruebas` (los siete casos ejecutables) |
| `:app` | `com.example.uikit.demo` | Doce escenas: la última es **Pruebas**, con la cobertura del plan, el botón de ejecutar la tanda y el de guardar el informe |
| pruebas | `com.example.uikit` (androidTest) | Las 41 anteriores + `PruebasSistemaTest` con 7 pruebas nuevas — **48 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto: la salida `OK (48 tests)` ·
`Time: 23.719` es la que devolvió `am instrument` en el emulador, la línea base
`5abf86fa…1374829` es la huella SHA-256 de una ejecución real, y las cifras del informe (755 ms y
51.552 KB para un año completo de registros) están leídas del fichero que la app escribió en
`filesDir` y se sacó con `run-as`.

## Resultado de aprendizaje

**RA8. Evalúa el funcionamiento de aplicaciones diseñando y ejecutando pruebas** — la unidad lo
trabaja **completo**: los siete criterios de evaluación, de la `a` a la `g`.

| CE | Qué se hace en la unidad |
| -- | ------------------------ |
| a | El plan escrito en `Pruebas.kt`: casos, tipos, herramientas y presupuestos |
| b | P-01 (unitaria) y P-02 (integración de filtrar y resumir), con banco de datos doblado |
| c | P-03: huella SHA-256 del CSV contra la línea base, con una prueba paralela que exige que la regresión muerda |
| d | P-04 y P-05: tres niveles de volumen y cien pasadas con control de degradación |
| e | P-06: auditoría de los ficheros que la app escribe en el teléfono |
| f | P-07: memoria del montículo y tiempo del lote de un año completo |
| g | El informe Markdown generado por la app, con lo manual sin ejecutar |

## Relación con otros módulos

Las pruebas consumen todo el módulo: el informe que se filtra y se resume es el de la **UD8**, los
ficheros que audita la prueba de seguridad son los de la **UD9**, y la firma y el versionado de la
**UD10** son los que deciden qué build se prueba. En **Entornos de Desarrollo** (EDES) se trabaja
la depuración y el ciclo construir-probar; en **Programación** (PROG) aparecen los dobles y el
cubrimiento; en **Despliegue de aplicaciones web** (DAW) la misma idea llega como integración
continua: si la tanda no se ejecuta sola en cada commit, la mitad de su valor se pierde.
