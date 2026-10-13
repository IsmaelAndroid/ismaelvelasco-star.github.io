---
title: "Castillo de Luna 12: Realización de pruebas y depuración"
description: Caja negra y caja blanca, depuración con Logcat y el depurador de Android Studio, pruebas de regresión y JUnit sobre un carrito de la compra.
summary: "La práctica de pruebas del 24/25 con las herramientas actuales: Logcat, breakpoints, suite de regresión y tests del carrito."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/bug-check"
permalink: /di/castillo/p12
categories:
    - DI
tags:
    - DI
    - Pruebas
    - Depuración
    - JUnit
---

# Práctica Castillo 12: Realización de pruebas

La original clasificaba pruebas, cazaba errores y montaba regresión sobre proyectos Java.
Aquí igual, pero con las armas de Android Studio: **Logcat**, **breakpoints** y la suite de
tests que ya dominas de la práctica Castillo 5.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Clasificación de pruebas

Investiga y clasifica las pruebas en función de **caja negra** y **caja blanca**. Aporta
ejemplos prácticos de cada tipo y justifica su importancia en el desarrollo de software.
Utiliza **Genially** para documentarlo todo.

### 2. Identificación y corrección de errores

Utiliza un proyecto de prácticas anteriores al que le metas (o ya tenga) **un error de
lógica** y **un error de ejecución**:

1. Identifica los errores utilizando Android Studio (**Logcat** para la excepción,
   **breakpoints** y *Evaluate Expression* para la lógica).
2. Describe cómo se detectaron y corrigieron (capturas del Logcat con el stack trace y del
   breakpoint en acción).
3. Documenta el proceso de depuración paso a paso.

### 3. Pruebas de regresión

Dado un ejercicio de otra práctica (por ejemplo, la calculadora):

1. Añade una funcionalidad nueva que modifique partes del código existente (p. ej., el
   botón `%` de porcentaje, o la memoria `M+`).
2. Ejecuta la **suite de regresión** (los tests de Castillo 5 + los nuevos) para verificar
   que no se introdujeron errores nuevos.
3. Documenta los resultados obtenidos (ventana de resultados, todo verde).

### 4. Automatización de pruebas con JUnit

Utiliza JUnit para escribir pruebas unitarias de un programa que gestione un **carrito de
compras** (puedes apoyarte en la IA para crear la lógica con el modelo MVC; la interfaz
puede quedarse en consola o en Compose, lo que decidas).

1. Escribe casos de prueba para: **agregar producto**, **eliminar producto** y **calcular
   el precio total** (incluye cupones de descuento si te atreves).
2. Ejecuta las pruebas y documenta los resultados obtenidos.

## Consejos

- Error de ejecución fácil y pedagógico: `IllegalArgumentException` al dividir por cero sin
  proteger. Error de lógica: sumar donde había que multiplicar (la app no peta, pero da mal).
- Logcat con filtro `package:es.iesra` para no ahogarse en logs ajenos.
- Lenguaje técnico, capturas propias, código comentado línea a línea.

## Importante

Entrega en **PDF** con capturas válidas. Retraso = 1.
