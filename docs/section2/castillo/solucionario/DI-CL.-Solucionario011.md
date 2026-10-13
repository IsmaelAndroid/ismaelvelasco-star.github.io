---
title: "Solucionario Castillo 11"
description: Esquema del APK, flujo de firma con keystore, tabla APK vs AAB y pasos de Play Console.
summary: "Solución completa de la práctica Castillo 11."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s11
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 11

## Ejercicio 1 — Qué hay dentro de un APK

```
miapp-release.apk
├── AndroidManifest.xml        ← DNI de la app: permisos, actividades, versión
├── classes.dex                ← el Kotlin COMPILADO a bytecode Dalvik
├── res/                       ← layouts, imágenes y strings YA COMPILADOS
├── resources.arsc             ← índice de recursos por configuración (idioma, densidad)
├── lib/                       ← bibliotecas nativas .so por arquitectura (arm64, x86...)
├── assets/                    ← ficheros crudos empaquetados
├── META-INF/                  ← las FIRMAS (quién soy y que nadie me ha tocado)
└── kotlin/                    ← metadatos de Kotlin
```

**APK vs AAB:** el APK es un todo-para-todos (lleva recursos de todos los idiomas y todas
las densidades); el **AAB** es un formato de publicación que Google **optimiza por
dispositivo**: genera un APK distinto para cada móvil con solo lo que necesita. Por eso la
tienda pide AAB: descargas más pequeñas para el usuario final.

## Ejercicio 2 — Generar APKs

**Debug** (firma automática con la clave de depuración de Android Studio):

```
Build → Build App Bundle(s)/APK(s) → Build APK(s)
→ app/build/outputs/apk/debug/app-debug.apk
→ arrastrar al emulador para instalar
```

**Release firmado**:

```
Build → Generate Signed App Bundle / APK
→ APK → Create new… keystore (ruta FUERA del repo, contraseñas fuertes)
→ Release → (v1 ✔ v2 ✔) → Finish
→ app/build/outputs/apk/release/app-release.apk
```

`build.gradle.kts` del proyecto serio:

```kotlin
val ks = Properties().apply {
    load(rootProject.file("key.properties").inputStream())   // ¡en .gitignore!
}
android {
    signingConfigs {
        create("release") {
            storeFile = file(ks["storeFile"] as String)
            storePassword = ks["storePassword"] as String
            keyAlias = ks["keyAlias"] as String
            keyPassword = ks["keyPassword"] as String
        }
    }
    buildTypes { release { signingConfig = signingConfigs.getByName("release") } }
}
```

**Si pierdes el keystore:** no puedes actualizar la app (misma firma = misma identidad);
habría que publicarla como app nueva con otro nombre de paquete, perdiendo usuarios y
valoraciones. Por eso existe **Play App Signing**: Google custodia la clave real y tú
mantienes una de subida.

## Ejercicio 3 — Sideload vs Play Store

| Criterio | APK directo (sideload) | Play Store (AAB) |
| -------- | ---------------------- | ---------------- |
| Actualizaciones | manuales, a mano | automáticas |
| Confianza | "orígenes desconocidos" | revisión de Play |
| Permisos | sin control | declarados y auditados |
| Rollout | inmediato, sin filtros | etapas (0%→100%) |
| Cuándo usarlo | pruebas internas, aulas, kioscos | cualquier app de producción |

En clase hacemos sideload todo el trimestre (arrastar el APK al emulador); para producción,
tienda.

## Ejercicio 4 — Firma

`keytool -genkey -v -keystore mi-clave.jks -keyalg RSA -keysize 2048
-validity 10000 -alias mi-alias`

Verificación:

```
gradlew :app:validateSigningRelease
apksigner verify --print-certs app-release.apk
→ Signer #1 certificate DN: CN=Ismael Velasco, O=IESRA...
```

El dispositivo Android verifica en cada instalación/actualización que la firma coincida con
la de la app ya instalada: si no coincide, la instalación se rechaza. La firma ES la
identidad.

## Ejercicio 5 — Play Console (pista de pruebas internas)

1. Play Console → *Testing → Internal testing → Create release*.
2. Subir el **AAB** (`Build → Generate Signed Bundle`).
3. Añadir testers por correo (lista de correo o G- Mail del grupo).
4. Rollout → el enlace `https://play.google.com/console/.../internal-testing` se comparte
   y los testers instalAN desde Play como una app normal.
5. Documentar: cada pantalla del asistente con su propósito (firmas, notas de la versión,
   lista de testers).
