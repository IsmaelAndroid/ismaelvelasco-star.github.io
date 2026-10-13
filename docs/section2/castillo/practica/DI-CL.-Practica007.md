---
title: "Castillo de Luna 7: Usabilidad II — pautas de diseño"
description: Optimización del formulario de la práctica 3 y diseño de un gestor de tareas aplicando pautas y patrones Material.
summary: "La práctica de pautas de diseño del 24/25, traducida de menús de escritorio a patrones móviles: app bar, navegación, contextuales, Snackbar."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/palette"
permalink: /di/castillo/p7
categories:
    - DI
tags:
    - DI
    - Usabilidad
    - Material Design
---

# Práctica Castillo 7: Usabilidad II — Pautas de diseño

La original optimizaba el formulario de **Scene Builder** y diseñaba un gestor de tareas con
**Justinmind** pensando en menús de escritorio (Archivo, Edición...). El móvil tiene sus
propios estándares — **Material Design** — y esta práctica entrena exactamente eso: reconocer
una interfaz que no los cumple y llevarla a patrones que el usuario ya conoce.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Optimización de tu formulario

Optimiza el **formulario de registro** de la práctica Castillo 3. Para la optimización debes
aplicar al menos un cambio por cada defecto, dejando capturas del **antes** y el **después**
y explicando **qué pauta** has aplicado:

1. La navegación no es coherente con los estándares: reorganízala para que sea intuitiva
   (título en la app bar, acción principal accesible con el pulgar, botón destructivo
   separado del principal).
2. No se utilizan los controles adecuados para ciertas acciones: cambia los controles para
   que se adapten mejor a su función (fecha → selector de fecha; profesión → desplegable;
   DNI → teclado numérico con `KeyboardOptions`).
3. Los colores y las fuentes son poco legibles: mejora el diseño visual apoyándote en el
   **tema Material** (roles de color, `typography` con jerarquía) para que sea claro y
   profesional.
4. Los mensajes emergentes son confusos o incompletos: mejora su redacción y su componente
   (`Snackbar` para avisos no bloqueantes, `AlertDialog` solo para confirmaciones críticas).

### 2. Diseña una app de gestión de tareas personales

Diseña el prototipo en **Figma** (o maquétalo directamente en Compose si te atreves).
Debe permitir al usuario:

1. Agregar una tarea nueva.
2. Marcar tareas como completadas.
3. Filtrar tareas por estado (pendiente / completada).
4. Acceder a una pantalla de ayuda.

Instrucciones:

1. **Crear navegación estándar**: implementar la estructura con app bar superior con título
   y menú, y una **navegación inferior** con las opciones: *Tareas*, *Filtro* y *Ayuda*.
   Cada opción debe incluir acciones relevantes.
2. **Crear un menú contextual**: al mantener pulsada una tarea, muestra las opciones
   *Marcar como completada* o *Eliminar*.
3. **Distribuir las acciones de forma coherente**: usa un **botón flotante (FAB)** para la
   acción más usada (agregar tarea) y accesos rápidos al filtro.
4. **Diseño visual legible**: fondo claro, tipografía con jerarquía (título / cuerpo /
   etiqueta), botón principal con el color primario del tema y texto contrastado.
5. **Mensajes claros**: al eliminar una tarea muestra "¿Seguro que deseas eliminar esta
   tarea? Esta acción no se puede deshacer." con *Eliminar* / *Cancelar*.

## Consejos

- Material Design 3 es LA referencia: `m3.material.io` tiene cada patrón con su nombre.
- El antes/después en dos capturas lado a lado vende más que diez párrafos.
- Lenguaje técnico y capturas propias. Retraso = 1 (esto no cambia).

## Importante

Entrega en **PDF** con capturas válidas y el enlace al prototipo de Figma. Retraso = 1.
