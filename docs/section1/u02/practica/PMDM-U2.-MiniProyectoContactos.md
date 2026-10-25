---
title: "Práctica 2.1 - Tu libreta, la nuestra (mini-proyecto)"
description: "Mini-proyecto de la UD2: construir una app de contactos con ciclo de vida gestionado, intents y lista Lazy."
authors:
    - Ismael Velasco
date: 2026-10-25
icon: "material/tools"
permalink: /pmdm/unidad2/practica
categories:
    - PMDM
tags:
    - PMDM
    - Android
    - Intents
    - Compose
---
# Práctica 2.1 - Tu libreta, la nuestra (mini-proyecto)

**UD 2 · RA2 (CE a, b, e, f) · 2 sesiones de 55 min · entregable: proyecto Android + 5 capturas**

La teoría del tema 2 usa una libreta de contactos (FumetoContactos). Tu trabajo es construir **la tuya propia**: misma anatomía, contenido y decisiones tuyas. Vale libreta de contactos, de peluquerías, de grupos de WhatsApp del barrio — lo que sea, siempre que sean **fichas con nombre + teléfono**.

## Qué tiene que tener (checklist de entrega)

| # | Requisito | Qué demuestra |
|---|---|---|
| 1 | App con `Scaffold`: `TopAppBar` con **tu** título y FAB | 2.3 |
| 2 | `LazyColumn` con **al menos 6 fichas** tuyas, con `key` | 2.3 |
| 3 | Cada ficha: pulsar toda la tarjeta abre la pantalla de detalle con **intent explícito + extras** | 2.2 |
| 4 | En el detalle: botón **llamar** (ACTION_DIAL) y botón **compartir** (ACTION_SEND con chooser) | 2.2 |
| 5 | Un interruptor o favorito cuyo estado **sobreviva a girar el emulador** (Bundle) | 2.2 |

## Orden de trabajo (los minutos cuentan)

**Sesión 1 (55 min):**
1. (10) Proyecto nuevo Android (plantilla Empty Activity, Compose): renombra app y título. Sin plantilla KMP: esto es app Android a secas.
2. (15) `Scaffold` + barra + lista Lazy con 6 datos hardcodeados (data class + lista).
3. (15) Tarjeta pulsable → segunda Activity con extras (claves en `companion object`, lectura con valor por defecto).
4. (15) Botón llamar y botón compartir en el detalle. Capturas 1-4.

**Sesión 2 (55 min):**
5. (15) Favorito con switch + guardado en `onSaveInstanceState`. Prueba de rotación.
6. (15) Tu toque personal (elige uno): alta con formulario estilo 2.3, o `intent-filter` propio que abra tu app desde una URL, o notificación con `PendingIntent`.
7. (15) Limpieza: sin código muerto, comentarios donde manden las decisiones, versiones: proyecto subido a vuestro repositorio Git con mensaje por día de trabajo.
8. (10) Autoevaluación con la tabla de abajo. Captura 5.

## Entregable (5 capturas + enlace al repo)

1. Lista con tus 6 fichas renderizadas.
2. Detalle de una ficha abierto con datos recibidos por extras.
3. Marcador del sistema con un teléfono tuyo listo para llamar.
4. Chooser de compartir con tu texto.
5. La app **tras rotar** con el favorito intacto (se ve el antes y el después en la misma imagen o en dos).

## Criterios de corrección (sobre 10)

| Bloque | Peso |
|---|---|
| Estructura app: Manifest, Activities, Scaffold | 2,0 |
| Ciclo de vida y Bundle (el favorito sobrevive) | 2,5 |
| Intents: explícito con extras + 2 implícitos funcionando | 3,0 |
| Lista Lazy con clave + interacción tarjeta | 1,5 |
| Higiene: repo Git, sin código muerto, capturas claras | 1,0 |

## Consejos (y una advertencia)

- Prueba cada intent **nada más escribirlo** en el emulador: un `ActivityNotFoundException` a tiempo te ahorra una hora.
- Las extras se leen con valor por defecto (`?: "sin datos"`): el emulador siempre puede lanzar tu Activity sin ellas.
- `remember` ≠ Bundle. Si giras y pierdes el favorito, releer el Código 4 del 2.2.
- Copiar de la teoría está permitido; no entender lo copiado, no: la defensa oral existe.

**Retraso = 1** punto por cada sesión de retraso sobre la fecha de entrega.
