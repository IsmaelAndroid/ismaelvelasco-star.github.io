---
title: "Castillo de Luna 11: Distribución — APK, AAB y firma"
description: La práctica de JAR, Launch4j y JarSigner del 24/25, traducida al pipeline real de distribución Android.
summary: "Qué contiene un paquete de instalación Android, APK debug vs release, APK vs AAB, firma con keystore y despliegue en Play Console."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/package-variant-closed"
permalink: /di/castillo/p11
categories:
    - DI
tags:
    - DI
    - Distribución
    - APK
    - Firma
---

# Práctica Castillo 11: Distribución de aplicaciones

La original estudiaba el **JAR**, firmaba con **JarSigner** y montaba un instalador **EXE con
Launch4j**. El pipeline Android es calcado con otros nombres: el paquete es el **APK/AAB**, la
firma es el **keystore**, y el "instalador" es la **Play Store**. Quiien entiende uno,
entiende todos.

**Duración estimada:** 3 horas de aula.

## Ejercicios

### 1. Componentes de un paquete de instalación

Describe los componentes principales de un paquete de instalación Android (**APK**) y su
importancia para garantizar el correcto funcionamiento de la aplicación en cualquier
entorno. Ayúdate de **draw.io**.

Instrucciones:

1. Explica qué contiene un APK (dex, recursos, manifiesto, firmas, nativas...).
2. Da ejemplos de componentes necesarios: código compilado (`classes.dex`), recursos
   (`res/`), manifiesto (`AndroidManifest.xml`), bibliotecas nativas (`lib/`).
3. Identifica qué diferencia un APK de un **AAB** (App Bundle) y por qué Play Store pide
   el segundo.

### 2. Creación de un APK en Android Studio

Implementa un proyecto ya realizado en prácticas anteriores y genera un **APK** siguiendo
los pasos del tema.

Instrucciones:

1. Genera el **APK de depuración** (*Build → Build App Bundle(s)/APK(s) → Build APK(s)*) e
   instala la app en tu emulador.
2. Genera también un **APK firmado de release** (*Build → Generate Signed App Bundle/APK*),
   creando tu **keystore** propio. Documenta cada paso con capturas.
3. Explica qué pasa si pierdes el keystore (y por qué Google aconseja jugar con el
   *Play App Signing*).

### 3. Instaladores y su funcionamiento

Investiga y explica las diferencias entre distribuir por **APK directo (sideload)** y por
**Play Store (AAB + Play App Signing)**.

Instrucciones:

1. Define qué es el sideload y qué es la distribución por tienda.
2. Ventajas e inconvenientes de cada una (actualizaciones, confianza, permisos,
   "orígenes desconocidos").
3. Indica cuándo usarías una u otra en un proyecto real.

### 4. Firma de una aplicación

Usa el **keystore** para firmar tu APK de release.

Instrucciones:

1. Genera el keystore con el asistente de Android Studio (o `keytool` en línea de comandos).
2. Firma el APK y verifica la firma (*Settings → Build → APK Signature Scheme v2/v3*, o
   `apksigner verify` desde la terminal de Gradle).
3. Describe cómo verifica un dispositivo Android la autenticidad de la app firmada
   (misma firma = misma identidad; la firma es lo que permite actualizar una app instalada).

### 5. Publicación de prueba

Sube tu AAB a **Play Console** en la pista de **pruebas internas** (cuenta gratuita de
desarrollador no necesaria si usas la del centro) y comparte el enlace de prueba. Si no
tienes acceso a una consola, documenta el flujo completo con las capturas oficiales del
asistente y explica cada pantalla.

## Consejos

- El keystore y sus contraseñas NO se suben al repositorio. Nunca. Va en `.gitignore` y en
  tu caja fuerte digital.
- Un proyecto real: `key.properties` fuera de Git + `signingConfigs` leyéndolo.
- draw.io para el esquema del APK: queda genial y se tarda poco.

## Importante

Entrega en **PDF** con capturas válidas. Retraso = 1.
