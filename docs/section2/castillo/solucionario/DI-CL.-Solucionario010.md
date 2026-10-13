---
title: "Solucionario Castillo 10"
description: Plantilla de la tabla de pruebas, índice del manual de usuario y esquema de guía rápida.
summary: "Solución-guía de la práctica Castillo 10."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s10
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 10

## Ejercicio 1 — Genially (mapa de contenidos mínimo)

5 pantallas enlazadas desde un índice interactivo:

1. **Portada/índice**: los 4 tipos de ayuda, cada uno navegable.
2. **Ayuda en línea**: qué es, ejemplo (la pestaña "?" de un IDE), cuándo usarla.
3. **Guía rápida**: arranque de 1 página; ejemplo: la tarjeta que sacan las apps al abrir
   por primera vez.
4. **Mensajes del sistema**: errores, confirmaciones y avisos (`Snackbar` vs `AlertDialog`).
5. **Tutoriales guiados**: coach marks y onboarding; ejemplo: la primera ejecución de
   cualquier app de banca.

## Ejercicio 2 — Documentación de pruebas (tabla modelo)

| # | Entrada | Esperado | Obtenido | Veredicto |
| - | ------- | -------- | -------- | --------- |
| 1 | 2 + 3 = | 5 | 5 | ✔ |
| 2 | 2 + 3 + 4 = | 9 | 9 | ✔ |
| 3 | 7 ÷ 0 = | "Error" (null) | "Error" | ✔ |
| 4 | CE tras 9 × 9 | reinicia | reinicia | ✔ |
| 5 | 123 tecleado | 123 | 123 | ✔ |

Adjunta: captura de la clase de test, captura de la ventana *Test Results* todo en verde, y
una captura del emulador con la operación 3 en pantalla. El punto extra es documentar un
**fallo encontrado y corregido**: los manuales de pruebas reales están llenos de eso.

## Ejercicio 3 — Manual + guía rápida

**Índice del manual (modelo):**

1. ¿Qué es esta app? (público y propósito)
2. Primera pantalla: la lista (captura con flechas a cada elemento)
3. Añadir una tarea (paso a paso con capturas)
4. Marcar como completada
5. Filtrar pendientes
6. Eliminar (con la confirmación explicada)
7. Preguntas frecuentes (3-4 reales)
8. Créditos y versión

**Guía rápida (1 página):**

```
   ① Escribe y pulsa ＋  →  nueva tarea
   ② Toca el cuadrado   →  completada
   ③ Icono embudo       →  ver solo pendientes
   ④ Mantén pulsado     →  eliminar
```

Cuatro gestos, cuatro iconos, cero párrafos. Si un abuelo no la entiende, no es rápida.
