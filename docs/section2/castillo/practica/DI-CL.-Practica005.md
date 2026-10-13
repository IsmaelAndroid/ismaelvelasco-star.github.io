---
title: "Castillo de Luna 5: Pruebas unitarias con JUnit"
description: La práctica de explotación de componentes del 24/25, ahora con JUnit corriendo dentro de Android Studio contra tu calculadora y tu agenda.
summary: "Tests unitarios de la lógica de la calculadora y de un GestorDeContactos MVC con validación de correo y teléfono."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/test-tube"
permalink: /di/castillo/p5
categories:
    - DI
tags:
    - DI
    - JUnit
    - Testing
    - MVC
---

# Práctica Castillo 5: Explotación de componentes — JUnit

La original mandaba testear con **JUnit** la calculadora de JavaFX y construir una **agenda
electrónica** con MVC y su batería de pruebas. En Android Studio, JUnit ya vive dentro: los
tests unitarios corren en la JVM (sin emulador, rapidísimos) y separan el grano (lógica) de
la paja (interfaz).

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Testea tu calculadora

Realiza **pruebas unitarias con JUnit** que verifiquen que los métodos de tu clase
`Calculadora` (la de las prácticas 2 y 3) funcionan correctamente. Las pruebas deben cubrir
todo tipo de escenarios:

- Operaciones básicas y encadenadas (`2 + 3 + 4`)
- **Divisiones por cero** (comportamiento definido: ¿ devuelve null, NaN, lanza excepción ?)
- `CE` en mitad de una operación
- Números de varias cifras (`numero()` componiendo dígitos)
- Límites: números muy grandes, decimales

### 2. Agenda electrónica creada y testeada

Crea una app con interfaz Compose para gestionar contactos, con campos para **nombre,
apellido, teléfono y correo electrónico**, y botones para:

- Agregar un nuevo contacto
- Editar un contacto existente
- Eliminar un contacto
- Listar todos los contactos
- Exportar la lista de contactos a un fichero **XML** *(IA: podéis apoyaros en la IA para
  la serialización, pero debéis entender cada línea que entreguéis)*

Desarrolla con el modelo **MVC**:

- Una clase `Contacto` que represente un contacto con los campos mencionados.
- Una clase `GestorDeContactos` con métodos para: agregar, editar, eliminar y listar.

Implementa **pruebas unitarias con JUnit** para `GestorDeContactos` cubriendo:

- Agregar / editar / eliminar / listar
- Comprobar que el **correo tiene formato correcto**
- Comprobar que el **teléfono tiene formato correcto** (9 dígitos, empieza por 6/7/9)

Integra las pruebas en el proyecto y asegúrate de que **pasan todas** antes de entregar.

## Consejos

- Los tests van en `src/test/java/...` (carpeta verde). Botón derecho sobre la clase →
  *Generate → Test* te monta el esqueleto.
- Un `assert` por escenario. Si un test se llama `divisionPorCero_devuelveError`, cualquiera
  entiende qué protege.
- Lenguaje técnico, capturas de la ventana de resultados (todo verde), código comentado.

## Importante

Entrega en **PDF** con capturas válidas. Retraso = 1.
