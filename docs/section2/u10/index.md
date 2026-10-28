---
title: "UD 10 - Distribución y publicación"
description: "Índice de la unidad 10: preparar la aplicación para que salga del ordenador — variantes de compilación, firma con almacén de claves, versionado, APK frente a AAB, peso real y canales de distribución (RA7 completo)."
authors:
    - Ismael Velasco
date: 2026-10-28
icon: "material/package-variant-closed"
permalink: /di/unidad10/
categories:
    - DI
tags:
    - DI
    - Jetpack Compose
    - distribución
    - firma de aplicaciones
    - APK
    - AAB
    - Google Play
---

# UD 10 — Distribución y publicación

Hasta ahora la app ha vivido en el emulador, instalada por Gradle con la clave de depuración. Esta
unidad la saca a la calle: **almacén de claves propio**, build de publicación con R8, versionado,
APK y bundle, y los cuatro canales por los que puede salir. Y con una pantalla que se autocritica:
al pasar el control, la build de publicación **suspende dos de las seis comprobaciones** —pesa
37.899 KB y pide un permiso que no usa— porque un control que no falla nunca no controla nada.

## Contenido

- [Teoría 10.1 — Distribución y publicación](teoria/DI-U10.1.-DistribucionYPublicacion.md)

## Presentación

- [DI-U10.1 - Distribución y publicación](https://revilofe.github.io/slides/section6-di/DI-U10.1.-DistribucionYPublicacion.html)
## Qué aprenderás

- La diferencia real entre **debug y release**: firma, flag `DEBUGGABLE`, R8 y para qué sirve cada una.
- Crear un **almacén de claves** con `keytool` y entender qué dice `keytool -list` de él.
- Comprobar la firma desde fuera con `apksigner verify --print-certs` y por qué dos builds del mismo
  proyecto tienen dos huellas distintas.
- Que una actualización solo se instala si la firma coincide:
  <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`INSTALL_FAILED_UPDATE_INCOMPATIBLE` es una lección, no un accidente</span>.
- **Versionar** con `versionName` y `versionCode`, y qué pasa cuando no subes el entero.
- **APK y AAB**: qué es cada cosa y por qué el bundle pesa un 58 % menos de lo que descarga el móvil.
- Recortar el paquete con `localeFilters` y comprobar el resultado con `aapt2 dump badging`.
- Los **cuatro canales** (Play, F-Droid, sideload y tiendas de marca) y qué espera cada uno de ti.
- Montar un **control de distribución** como lista de comprobaciones, con umbrales escritos a la vista.

## Proyecto de la unidad

El mismo `IsmaelUIKit`, ahora con build de publicación firmada:

| Módulo | Paquete | Qué contiene ahora |
| ------ | ------- | ------------------ |
| `:ui` | `com.example.uikit` | Todo lo anterior + `Distribucion` (canales y control) y `Firma` (lo que el paquete instalado dice de sí mismo) |
| `:app` | `com.example.uikit.demo` | Once escenas: la última es **Distribución**, con la ficha de la build, los canales y el botón de pasar el control |
| `build.gradle.kts` | — | `signingConfigs` con `kit.jks`, `buildTypes.release` con R8, `localeFilters` y `buildConfig` |
| pruebas | `com.example.uikit` (androidTest) | Las 35 anteriores + `DistribucionTest` con 6 pruebas nuevas — **41 pruebas en verde** |

Todo el código de la teoría es el código real de ese proyecto: las tres capturas son de la build de
publicación instalada en el emulador, las cifras de peso son las de los ficheros generados
(51.386.451 / 38.809.364 / 21.303.705 bytes), las huellas son las que devolvió `apksigner` y la
salida de pruebas (`OK (41 tests)` · `Time: 18.957`) es la que devolvió `am instrument`.

## Resultado de aprendizaje

**RA7. Prepara aplicaciones para su distribución evaluando y utilizando herramientas específicas** —
la unidad lo trabaja **completo**: los ocho criterios de evaluación, de la `a` a la `h`.

| CE | Qué se hace en la unidad |
| -- | ------------------------ |
| a | El empaquetado de los componentes: las dos variantes y el envoltorio final |
| b | La personalización del «asistente»: ficha, icono adaptativo e idiomas declarados |
| c | Los paquetes generados con el entorno: `assembleRelease` y `bundleRelease` |
| d | Las herramientas externas: `keytool`, `apksigner` y `aapt2` |
| e | La firma digital con `kit.jks` y su verificación por huella SHA-256 |
| f | La instalación desatendida: `adb install` y los MDM de empresa |
| g | La desinstalación correcta: qué se va con la app y qué se queda |
| h | Los cuatro canales de distribución y lo que pide cada uno |

## Relación con otros módulos

La firma y el versionado afectan a todo lo anterior: la **UD9** documenta los ficheros que se pierden
al desinstalar, la **UD5** y la **UD6** aportan las pruebas que hay que pasar antes de publicar, y la
**UD11** convierte esta lista de comprobaciones en una estrategia de pruebas completa. En
**Despliegue de aplicaciones web** (DAW) el mismo concepto aparece con otro nombre: empaquetar, firmar
y elegir dónde publicar.
