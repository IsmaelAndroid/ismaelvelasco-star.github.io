---
title: "Castillo de Luna 2: Login, calculadora y formulario"
description: Las tres clásicas del 24/25 en Java Swing, reescritas en Jetpack Compose.
summary: "Pantalla de login con diálogo de resultado, calculadora completa con memoria de operación y formulario de datos personales con todos los tipos de campo."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/form-textbox"
permalink: /di/castillo/p2
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - Estados
    - Componentes
---

# Práctica Castillo 2: Clases y componentes

La práctica original pedía login, calculadora y formulario **en Java Swing**. Aquí van de nuevo,
en **Jetpack Compose**. Es la práctica donde se juega todo el temario de la UD2: componentes,
estado, contenedores y eventos.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Pantalla de login

Crear una pantalla de login con **dos campos** (`TextField`) para usuario y contraseña y **dos
botones**: *Entrar* y *Cancelar*. Si pulsa *Entrar* y el usuario es `Admin` y la contraseña
`123456`, se muestra un **diálogo** indicando que se ha logado correctamente. Con cualquier otro
dato, un diálogo igual indicando que la información de login es incorrecta. *Cancelar* limpia
los dos campos.

### 2. Calculadora

Crear una calculadora con botones para **cada dígito**, las operaciones **(+, −, ×, ÷, =)**, un
botón **CE** para empezar de cero, y una **caja de texto** que muestre el resultado actual y la
operación en curso. La lógica de cálculo debe ir en una **clase aparte** (`Calculadora`), no en
la interfaz.

### 3. Formulario de datos personales

Crear una interfaz para recoger los datos personales de una persona:

- Nombre y apellidos
- Fecha de nacimiento (campo de fecha)
- DNI
- Sexo (radio button)
- Correo electrónico
- Profesión (combo con 3 o 4 valores)
- Permitir comunicaciones comerciales (checkbox)
- Un botón **Guardar** y otro **Cancelar**

Al pulsar **Guardar** se muestran los datos introducidos en una sola cadena de texto
(diálogo o pantalla de resumen).

## Consejos

- La calculadora se hace en dos ficheros: `Calculadora.kt` (lógica) y `CalculadoraUI.kt`
  (interfaz). Os lo agradeceréis en la práctica 5, cuando haya que testearla.
- En el formulario, cada campo con **su** estado. Uno solo para todos = dolor.
- Lenguaje técnico y capturas propias; código comentado línea a línea.

## Importante

Entrega en **PDF** con capturas válidas (tu usuario, fecha del sistema, sin montajes).
Retraso = 1.
