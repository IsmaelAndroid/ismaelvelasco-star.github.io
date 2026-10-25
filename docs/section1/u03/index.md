---
title: "UD 3 - Programación en Android: gestión de datos"
description: "Unidad 3 de PMDM: navegación con datos, estado con ViewModel, Room, Retrofit y sesión. Cinco apartados dimensionados a las horas reales del módulo."
authors:
    - Ismael Velasco
date: 2026-10-25
icon: "material/database"
permalink: /pmdm/unidad3/
categories: PMDM
tags:
    - PMDM
    - Android
    - Navegación
    - Room
    - Retrofit
    - MVVM

toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

## **UD 3 - Programación en Android: gestión de datos**

La unidad donde la app deja de tener datos de mentira: navegación con argumentos, estado que sobrevive, base de datos local y servidor. Todo el código está ejecutado en emulador: las capturas son la prueba.

### 1. Normativa que respalda la unidad

Módulo **Programación Multimedia y Dispositivos Móviles** (0489), currículo vigente del **RD 405/2023** (BOE-A-2023-13221). Se trabajan del **RA2** los criterios **a)** estructura de clases, **e)** conexiones HTTP y HTTPS, **f)** conexiones con almacenes de datos garantizando la persistencia, **g)** pruebas de interacción en emulador, **i)** documentación de los procesos y **j)** permisos requeridos. El texto íntegro está en [`recursos/PMDM Normativa.txt`](../recursos/PMDM%20Normativa.txt).

### 2. Reparto de horas (esto es lo que da tiempo)

El módulo tiene **3 sesiones semanales** (≈2 h 45 min/semana). La unidad se cierra en **4 semanas**:

| Apartado | Contenido | Horas |
|---|---|---|
| 3.1 | Navegación con datos: argumentos, pila y deep links | 2 |
| 3.2 | Estado y formularios: ViewModel + StateFlow + validación | 2,5 |
| 3.3 | Room: persistencia local (entidades, DAO, Flow y migraciones) | 3 |
| 3.4 | Retrofit: red, DTOs y manejo de errores | 3 |
| 3.5 | Taller integrador: sesión con DataStore y pruebas | 2,5 |
| — | **Práctica evaluable** (entrega con capturas) | 3 |
| | **Total** | **16 h** |

> Los ocho apartados del borrador inicial (3.1 a 3.8) se han **fundido en cinco**: MVVM va con formularios, Hilt va con Room, y autenticación va con Retrofit. Así se da todo sin quedarse a medias.

### Teoría

- [3.1.- Navegación: grafo, argumentos y enlaces profundos](teoria/PMDM-U3.1.-NavegacionGrafoArgumentosYDeepLinks.md) — rutas con argumento tipado, control de la pila, valores por defecto y deep links.

- [3.2.- Estado y formularios: ViewModel, StateFlow y validación](teoria/PMDM-U3.2.-EstadoYFormularios.md) — dónde vive el estado, validación en el ViewModel y avisos con Snackbar.

- [3.3.- Room: persistencia local con entidades, DAO y Flow](teoria/PMDM-U3.3.-RoomPersistenciaLocal.md) — tabla, consultas, base de datos única, `Flow` y migraciones.

- [3.4.- Retrofit: red, DTOs y manejo de errores](teoria/PMDM-U3.4.-RetrofitRedYErrores.md) — peticiones HTTPS, `suspend`, `ignoreUnknownKeys`, permiso INTERNET y los tres estados de pantalla.

- [3.5.- Taller integrador: sesión con DataStore y pruebas](teoria/PMDM-U3.5.-TallerIntegradorSesionYPruebas.md) — sesión persistente, pruebas unitarias y repaso de la unidad.

### Prácticas

- Práctica 3.1 (mini-proyecto): «La agenda con datos» — 3 sesiones: tu app con navegación por argumentos, formulario validado, Room, petición HTTPS, sesión con DataStore y una prueba unitaria. Enunciado: [PMDM-U3.-AgendaConDatos](practica/PMDM-U3.-AgendaConDatos.md)

### Presentaciones

* [PM-U3 - Navegación, estado y red](https://revilofe.github.io/slides/section5-pmdm/PM-U3.-NavegacionEstadoYRed.html)

### Situación en el módulo

| UD | Contenido | Estado |
|---|---|---|
| UD 1 | Análisis de tecnologías móviles | ✅ publicada |
| UD 2 | IU con Compose | ✅ publicada |
| **UD 3** | **Gestión de datos (esta)** | **teoría completa** |
| UD 4 | Multimedia, testing y despliegue | en preparación |
| UD 5-6 | Motores de juegos y realidad aumentada | 2.º trimestre |
