---
title: "Castillo de Luna 9: Listados y exportación de datos"
description: La práctica de informes BIRT del 24/25 traducida a lo que hoy es un informe en Android: pantallas de listado filtradas y exportación a fichero.
summary: "Tres informes: listado simple, listado con parámetros de filtrado y exportación de datos de la agenda a fichero."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/file-table"
permalink: /di/castillo/p9
categories:
    - DI
tags:
    - DI
    - Informes
    - Exportación
---

# Práctica Castillo 9: Confección de informes (II)

La original pedía informes con **Eclipse BIRT** conectado a bases de datos de ejemplo. En
Android un "informe" es otra cosa: una **pantalla de listado con criterio** (qué se muestra,
en qué orden, con qué agrupación) y la **exportación** de esos datos a un fichero que pueda
abrir cualquiera (CSV, XML). La competencia es idéntica: transformar datos en información
presentable.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Informe simple: el listado de contactos

Crea una pantalla de **informe** con todos los contactos de tu agenda (práctica Castillo 5):
`LazyColumn` con `Card` por contacto, cabecera con el **número total**, y orden **alfabético
por apellido**. El contenido puede ser inventado si tu agenda está vacía.

### 2. Informe con parámetros

Añade al informe **parámetros de filtrado**:

- Un campo de texto que filtre por apellido **mientras se escribe**.
- Un desplegable con el criterio de ordenación (apellido / nombre / teléfono).
- Un contador que muestre cuántos resultados cumplen el filtro
  (`Mostrando X de N contactos`).

### 3. Exportación del informe

Exporta el listado **tal y como está filtrado** a un fichero **CSV** guardado en el
almacenamiento de la app, y muéstralo en pantalla en un `Text` (vista previa) o compártelo
con el `Intent` de compartir de Android. La exportación se hace desde el **modelo**
(`GestorDeContactos`), no desde el composable.

## Consejos

- Separad siempre: `GestorDeContactos.filtrar(...)` (modelo) y la pantalla que lo pinta.
- CSV = líneas con campos separados por `;`. Sin librerías externas se hace en 5 líneas.
- Capturas del informe con filtro activo y del CSV abierto en un editor de texto.

## Importante

Entrega en **PDF** con capturas válidas. Retraso = 1.
