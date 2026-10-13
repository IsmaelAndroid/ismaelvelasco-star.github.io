---
title: "Solucionario Castillo 6"
description: Plantillas de los tres tipos de test, rúbrica del contraejemplo y guion de las pruebas de experto/usuario.
summary: "Solución-guía de la práctica Castillo 6 (los tests son material de plantilla, no de respuesta única)."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s6
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 6

> Práctica de tipo "elaboración propia": el solucionario da la **plantilla correcta** de cada
> pieza y los puntos que se vigilan al corregir, no una respuesta única.

## Ejercicio 1 — Contraejemplos por característica

Plantilla por característica (la tabla que debe repetir el alumno):

| Campo | Qué se espera |
| ----- | ------------- |
| Característica | p. ej. *visibilidad del estado del sistema* |
| App y pantalla | nombre concreto + captura |
| Por qué falla | el usuario no sabe X porque la interfaz no muestra Y |
| Mejora | cambio concreto: "un `CircularProgressIndicator` en el botón mientras valida" |

Ejemplos correctos de fallos típicos por característica: botones sin estado de carga
(respuesta), mensajes de error genéricos ("Ha ocurrido un error"), campos sin etiqueta
(reconocimiento), flujo de 6 pantallas para una compra (eficiencia), iconografía inventada
(convenciones), sin deshacer (control del usuario), ayuda en un PDF externo (ayuda).

## Ejercicio 2 — Test de satisfacción (plantilla mínima)

1. Captura de la interfaz (imagen al inicio del formulario).
2. "Valora de 1 a 5 tu satisfacción general con la app" (escala lineal).
3. "¿La usarías por tu cuenta aunque no fuera obligatoria?" (Sí / No / Tal vez → uso voluntario).
4. "¿Qué mejorarías?" (párrafo abierto → caja de comentarios).
5. "¿Qué es lo que más te ha gustado?" (párrafo abierto).

> Métrica clave para comparar alumnos: la **escala 1-5** (media del grupo) y el **% de uso
> voluntario**.

## Ejercicio 3 — Test de efectividad (calculadora)

Preguntas tipo, una por métrica:

- "¿Conseguiste calcular 2+3+4 en el primer intento?" (Sí / No → tareas por intento)
- "¿Consultaste ayuda, apuntes o a un compañero?" (Nunca / 1 vez / Varias)
- "¿Cuántas funciones de la calculadora sabes usar ya?" (todas / casi todas / pocas)
- "¿Cuántos errores de dedo cometiste?" (0 / 1-3 / más de 3 → errores tolerados)

## Ejercicio 4 — Test de eficiencia (agenda)

- "Minutos hasta dar de alta tu primer contacto" (número)
- "Minutos para volver a eliminar un contacto una semana después" (número → reaprendizaje)
- "¿Tu primer intento funcionó?" (Sí / No → eficiencia relativa al primer intento)
- "¿Alguna acción te falla sistemáticamente?" (texto → errores persistentes)

## Ejercicio 5 — Experto y usuario (guion)

**Evaluación de alto nivel (experto):** recorrer la heurística completa con el checklist en
mano, sin tocar el móvil: jerarquía visual, coherencia de componentes Material, mensajes,
prevención de errores. Salida: lista de hallazgos ordenados por gravedad.

**Evaluación al detalle (usuario):** darle 3 tareas concretas ("da de alta este contacto",
"borra la Pista 7", "calcula 12×12") y observar en silencio, cronómetro en mano. Anotar
titubeos, retrocesos y atajos descubiertos. Salida: tiempos + frases literales del usuario.

**Pruebas razonables, factibles y específicas:** cada una con tarea, métrica y umbral
("alta de contacto < 60 s en 2º intento").

## Ejercicio 6 — Test de uso natural (solo diseñarlo)

Escenario: "Has llegado a casa y quieres apuntar 'comprar regalos' y borrar 'llamar al
fontanero'". El formulario NO da instrucciones de la app: solo plantea el escenario y
pregunta después por qué hizo, qué esperaba y qué le molestó. Con 5-8 ítems basta.
