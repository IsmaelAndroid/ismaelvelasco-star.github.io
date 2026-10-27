---
title: "UD 8 - Confección de informes"
description: "Índice de la unidad 8: el informe dentro de la aplicación — estructura, filtros, cálculos, gráficos propios y exportación real a PDF y CSV con las herramientas que se usan hoy en las empresas (RA5 completo)."
authors:
    - Ismael Velasco
date: 2026-10-27
icon: "material/chart-box"
permalink: /di/unidad8/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - informes
    - PDF
    - CSV
    - gráficos
---

# UD 8 — Confección de informes

Un informe no es un adorno al final de la aplicación: es la pantalla donde alguien mira cuánto se ha
gastado y decide. Esta unidad construye el **informe incrustado** del `IsmaelUIKit` —filtros por
categoría y por quincena, tabla de doce filas, totales, gráfico de barras y exportación a **PDF y
CSV**— con una sola función que hace la cuenta, y lo prueba con seis pruebas automáticas. El PDF no lo
hace una librería de pago: lo dibuja la app con `PdfDocument`, que viene en Android.

## Contenido

- [Teoría 8.1 — Confección de informes](teoria/DI-U8.1.-ConfeccionDeInformes.md)

## Qué aprenderás

- **Estructurar** un informe en siete partes: cabecera, filtros, cabecera de tabla, filas, totales,
  gráfico y pie.
- De dónde salen los datos (base de datos, API, CSV, la propia app) y por qué
  <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">el filtro es un dato, no un `if` suelto</span>.
- **Calcular** el pie: recuento, suma, media y total por categoría, y por qué la media de medias está
  mal y por qué el dinero no se suma con `Double`.
- Dibujar un **gráfico de barras** a mano con Canvas: escala, proporción y etiquetas accesibles.
- Pasar del **asistente** (JasperReports, Power BI, Looker Studio) al código propio, y modificarlo:
  PDF con `PdfDocument` paginado y CSV con el separador que espera una hoja de cálculo en español.
- **Incrustar** el informe en la app: el usuario filtra, ve el resultado y se lo lleva, sin servidor.
- Qué hace la **IA** en todo esto (escribe el código en segundos) y qué no decide (qué se cuenta, qué
  se filtra, qué significa cada cifra).

## Proyecto de la unidad

El mismo `IsmaelUIKit`, ampliado con la escena 9:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | Todo lo anterior + `Informe` (modelo, filtro y resumen), `GraficoBarras` y `ExportaInforme` (PDF y CSV) |
| `:app` | `com.example.uikit.demo` | Nueve escenas: la última es el **informe de gastos** con sus filtros y su exportación |
| pruebas | `com.example.uikit` (androidTest) | Las 23 anteriores + `InformeTest` con 6 pruebas nuevas — **29 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto: las tres capturas son del emulador, el
CSV publicado es el fichero que escribió la app y la salida de pruebas (`OK (29 tests)` ·
`Time: 32.171`) es la que devolvió `am instrument`.

## Resultado de aprendizaje

**RA5. Crea informes evaluando y utilizando herramientas gráficas** — la unidad lo trabaja **completo**:
los ocho criterios de evaluación, de la `a` a la `h`.

| CE | Qué se hace en la unidad |
| -- | ------------------------ |
| a | Las siete partes del informe y qué pregunta responde cada una |
| b | De la fuente de datos al informe, con y sin asistente |
| c | `FiltroInforme` y `filtrar()`: categoría y rango de días |
| d | `resumir()`: recuento, suma, media y total por categoría |
| e | `GraficoBarras` con Canvas y etiquetas accesibles |
| f | Qué genera un asistente de informes y con qué herramientas |
| g | `ExportaInforme`: PDF paginado con `PdfDocument` y CSV entrecomillado |
| h | La escena 9: informe incrustado con filtros y exportación |

## Relación con otros módulos

El informe se apoya en todo lo anterior: los **componentes** de la UD4 y el **estado** de la UD5 para
las tarjetas y los filtros, y los criterios de **accesibilidad** de la UD6 (las etiquetas del gráfico
son texto y llevan descripción). Lo que aquí se mide y se exporta es lo que la **UD11** convierte en
pruebas del sistema completo.
