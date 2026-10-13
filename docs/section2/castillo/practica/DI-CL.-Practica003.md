---
title: "Castillo de Luna 3: De layout XML a Compose"
description: La vieja práctica de generar interfaces desde XML (Scene Builder), traducida al salto real de hoy: leer layouts XML de Android clásico y reconstruirlos en Compose.
summary: "Tres interfaces descritas en XML que hay que reconstruir en Jetpack Compose reutilizando la lógica de la práctica 2."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/xml"
permalink: /di/castillo/p3
categories:
    - DI
tags:
    - DI
    - XML
    - Jetpack Compose
    - LazyColumn
---

# Práctica Castillo 3: De layout XML a Compose

En el 24/25 esta práctica iba de **Scene Builder**: pintabas el formulario y él te generaba el
FXML (el XML de la interfaz). La historia se repite en Android: durante una década las
interfaces fueron **layouts XML**, y de golpe llegó Compose y las interfaces se escriben en
Kotlin. Leer un XML de interfaz y saber **traducirlo a composables** es exactamente la
competencia que se evalúa aquí: la que necesitó todo desarrollador Android en la migración
real de 2021 en adelante.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Formulario desde XML

Dado este layout clásico, reconstruye la misma interfaz en Compose y adjunta capturas del
resultado en el emulador:

```xml
<LinearLayout orientation="vertical" padding="16dp">
    <EditText hint="Nombre completo"/>
    <EditText hint="Correo" inputType="textEmailAddress"/>
    <EditText hint="Contraseña" inputType="textPassword"/>
    <RadioGroup>
        <RadioButton text="Alumno"/>
        <RadioButton text="Profesor"/>
    </RadioGroup>
    <Button text="Registrar"/>
</LinearLayout>
```

### 2. Calculadora desde XML

Este XML describe la vieja calculadora de `LinearLayout` anidados. Reconstrúyela en Compose
**reutilizando la lógica de tu `Calculadora` de la práctica Castillo 2** (¡no la reescribas!):

```xml
<LinearLayout orientation="vertical">
    <TextView id="display" textSize="32sp" gravity="right"/>
    <LinearLayout orientation="horizontal"> <Button text="7"/> <Button text="8"/> <Button text="9"/> <Button text="÷"/> </LinearLayout>
    <LinearLayout orientation="horizontal"> <Button text="4"/> <Button text="5"/> <Button text="6"/> <Button text="×"/> </LinearLayout>
    <LinearLayout orientation="horizontal"> <Button text="1"/> <Button text="2"/> <Button text="3"/> <Button text="−"/> </LinearLayout>
    <LinearLayout orientation="horizontal"> <Button text="CE"/> <Button text="0"/> <Button text="="/> <Button text="+"/> </LinearLayout>
</LinearLayout>
```

### 3. Lista de tareas

Reconstruye en Compose esta pantalla clásica de `ListView`:

```xml
<LinearLayout orientation="vertical" padding="12dp">
    <EditText id="nuevaTarea" hint="Escribe una tarea..."/>
    <Button text="Añadir"/>
    <ListView id="listaTareas"/>
</LinearLayout>
```

Requisitos funcionales:

- Los usuarios podrán **agregar tareas** a la lista y **eliminarlas** una vez completadas
  (mantén pulsada la tarea para eliminarla, o un icono de borrado por fila).
- La interfaz debe mostrar un campo de texto para nuevas tareas, un botón para agregarlas y
  la lista de tareas agregadas.

## Consejos

- Tabla de traducción mental: `LinearLayout vertical` → `Column`, `horizontal` → `Row`,
  `padding` → `Modifier.padding`, `gravity` → `Alignment`, `ListView` → `LazyColumn`,
  `hint` → `label`, `inputType` → `KeyboardOptions`.
- **¡Reutilizad la lógica ya desarrollada!** (lo pedía la práctica original y sigue siendo el
  punto que más peso tiene en la nota).
- Capturas del XML original junto a tu resultado en el emulador: se ve el antes y el después.

## Importante

Entrega en **PDF** con capturas válidas. Retraso = 1.
