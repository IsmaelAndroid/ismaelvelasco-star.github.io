---
title: "Práctica 3.1 - La agenda con datos (mini-proyecto)"
description: "Mini-proyecto de la UD3: agenda con navegación por argumentos, formulario validado en el ViewModel, Room, petición HTTPS con estados, sesión con DataStore y una prueba unitaria."
authors:
    - Ismael Velasco
date: 2026-10-25
icon: "material/tools"
permalink: /pmdm/unidad3/practica
categories:
    - PMDM
tags:
    - PMDM
    - Android
    - Room
    - Retrofit
    - DataStore
    - MVVM
---
# Práctica 3.1 - La agenda con datos (mini-proyecto)

**UD 3 · RA2 (CE a, e, f, g, i, j) · RD 405/2023 · 3 sesiones de 55 min · entregable: proyecto Android + 7 capturas + prueba unitaria en verde**

En la UD2 construiste la interfaz de una libreta. Ahora le metes **datos de verdad**: se guardan en el móvil, se piden a un servidor y la sesión sobrevive a cerrar la app. Igual que entonces, la app es **tuya**: puede ser una agenda de contactos, de clientes de tu tienda, de equipos de fútbol del barrio o de recambios del taller. Lo que no es negociable es la anatomía.

## Qué tiene que tener (checklist de entrega)

| # | Requisito | Qué demuestra | Apartado |
|---|---|---|---|
| 1 | Grafo de navegación con **dos destinos** y ruta con argumento (`detalle/{id}`) | la ruta es el contrato | 3.1 |
| 2 | Un **deep link propio** que abra el detalle de una ficha concreta | entrada externa a la app | 3.1 |
| 3 | Formulario de alta con **dos validaciones** cuyo error se pinta bajo el campo | validación como lógica de negocio | 3.2 |
| 4 | El estado del formulario **sobrevive a girar el emulador** | ViewModel, no `remember` | 3.2 |
| 5 | **Room**: entidad con `@PrimaryKey(autoGenerate = true)`, DAO con `Flow` y base de datos con **una sola instancia** | persistencia local | 3.3 |
| 6 | La lista se pinta **desde la base de datos** y sigue ahí tras cerrar la app | datos en disco | 3.3 |
| 7 | Una petición **HTTPS** con Retrofit que muestre **cargando**, **error** y **datos** | los tres estados de la red | 3.4 |
| 8 | **Sesión** guardada con DataStore que sigue activa tras cerrar la app | persistencia de claves | 3.5 |
| 9 | **Una prueba unitaria** de la validación, en verde con `gradlew test` | lógica separada de la UI | 3.5 |
| 10 | Permiso `INTERNET` declarado y README con instrucciones | permisos y documentación | CE i), j) |

## Orden de trabajo (los minutos cuentan)

**Sesión 1 (55 min) — navegación y formulario**
1. (10) Proyecto nuevo (Empty Activity, Compose). Paquete y título tuyos.
2. (15) `NavHost` con `inicio` y `detalle/{id}` con `navArgument` de tipo `IntType`. Navegar desde la tarjeta y volver con `popBackStack()`.
3. (10) Lectura del argumento **con valor por defecto** y aviso si la ficha no existe.
4. (20) Pantalla de alta: `ViewModel` con `MutableStateFlow`, dos reglas de validación y errores visibles. Capturas 1-3.

**Sesión 2 (55 min) — Room y red**
5. (20) Entidad + DAO + base de datos singleton. Cambia la lista de memoria por la lista del `Flow`. Capturas 4 y 5.
6. (25) Interfaz Retrofit + DTO `@Serializable` + pantalla con estados `cargando`/`error`/`datos`, con `try/catch/finally`. Capturas 6a y 6b (éxito y error).

**Sesión 3 (55 min) — sesión, pruebas y entrega**
7. (15) DataStore con el token de sesión: guardar, observar y borrar. Captura 7 (antes y después de cerrar la app).
8. (15) Prueba unitaria de la validación. Debe salir `BUILD SUCCESSFUL` con `failures="0"`.
9. (15) README con: cómo se compila, permisos usados, capturas y decisiones tomadas. Última revisión del checklist.

## Entregable

- Enlace al **repositorio** con el código comentado línea a línea.
- **7 capturas** con nombre descriptivo:
    1. `01-detalle.png` — el detalle con el argumento recibido.
    2. `02-deeplink.png` — el detalle abierto por tu deep link.
    3. `03-validacion.png` — el formulario con los dos errores a la vez.
    4. `04-room-3.png` — tres fichas leídas de Room.
    5. `05-room-persiste.png` — lo mismo tras cerrar la app.
    6. `06-red.png` — la lista remota cargada **y** la pantalla con error controlado (puede ser un montaje de dos).
    7. `07-sesion.png` — sesión activa **y** sigue activa tras cerrar la app.
- Salida de `gradlew test` pegada en el README (o captura del informe).

## Criterios de corrección (sobre 10)

| Apartado | Peso |
|---|---|
| Navegación con argumento y deep link | 1,5 |
| Formulario con validación en el ViewModel y estado que sobrevive | 2 |
| Room: entidad, DAO con `Flow`, singleton y persistencia demostrada | 2,5 |
| Retrofit con los tres estados y `finally` que quita la carga | 2 |
| DataStore de sesión | 1 |
| Prueba unitaria en verde | 0,5 |
| Código comentado y README | 0,5 |

**Descuentos automáticos:** captura que no se corresponde con el código entregado (−1); petición de red sin permiso `INTERNET` o sin captura de error (−0,5); `fallbackToDestructiveMigration()` sin justificar (−0,5).

## Consejos (y una advertencia)

- **Pide primero el permiso `INTERNET`.** Sin él la petición falla con `EPERM (Operation not permitted)` en `getaddrinfo` y parece un problema de red: es de permisos.
- Si usas **AGP 9**, añade `android.disallowKotlinSourceSets=false` en `gradle.properties` antes de meter Room con KSP.
- `baseUrl` **termina en `/`** y la ruta del `@GET` **no empieza** por `/`.
- Pon el `cargando = false` en el `finally` o la rueda de carga no para nunca.
- **No vale** poner los datos en una lista global y decir que es Room: la prueba es cerrar la app desde el lanzador y volver a entrar.

**Advertencia:** el código de las capturas tiene que estar en el repo. Si la captura muestra una pantalla que no existe en tu código, se cuenta como no entregada.
