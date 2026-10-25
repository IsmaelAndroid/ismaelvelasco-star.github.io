---
title: "UD 1 - 1.1 Análisis de tecnologías para dispositivos móviles"
description: Dispositivos móviles, redes, sistemas operativos, tecnologías de desarrollo, emuladores, estructura de apps, ciclo de vida y herramientas del administrador de aplicaciones.
summary: Panorámica completa de las tecnologías implicadas en el desarrollo de aplicaciones para dispositivos móviles, desde el hardware y las redes hasta Android Studio, los emuladores y el ciclo de vida de una aplicación.
authors:
    - Ismael Velasco
date: 2026-10-08
icon: "material/file-document-outline"
permalink: /pmdm/unidad1/1.1
categories:
    - PMDM
tags:
    - PMDM
    - Dispositivos móviles
    - Android
    - Emuladores
    - Ciclo de vida

# Relacionado con la tabla de contenidos
toc: true
toc_label: "Contenido"
toc_icon: "file-code"
---

## 1.1. Análisis de tecnologías para dispositivos móviles

<div style="background-color:#a5d6a7; border-radius:8px; padding:10px 14px; margin:14px 0;">
<strong>🟢 Leyenda de repaso obligatorio:</strong> los textos con <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">este fondo verde</span> son imprescindibles: hay que repasarlos <strong>cada semana</strong>, porque entrarán en todas las evaluaciones del módulo.</div>

!!! abstract "Idea principal"
    Antes de programar una aplicación móvil hay que conocer el terreno de juego: qué dispositivos existen, qué redes usan, cómo se organizan sus sistemas operativos y qué tecnologías de desarrollo hay disponibles. Elegir bien la tecnología condiciona el rendimiento, el coste y el alcance de la app.

Un dispositivo móvil es, en la práctica, un ordenador de bolsillo diseñado para funcionar con batería, conexión inalámbrica y pantalla táctil. Desde los PDA y los primeros smartphones con teclado físico se ha pasado a terminales con pantallas táctiles de alta resolución, CPU multinúcleo, GPU potentes y múltiples sensores. Las mejoras en los dispositivos y en las redes ha posibilitado la aparición de un gran mercado de aplicaciones, y para nutrir ese mercado se han creado numerosos entornos de desarrollo.

El objetivo de este tema no es memorizar definiciones aisladas, sino entender qué podemos exigir a cada tecnología y saber aplicarla en situaciones cercanas al trabajo real.

| Código | Descripción |
|--------|-------------|
| RA 1   | Aplica tecnologías de desarrollo para dispositivos móviles evaluando sus características y capacidades. |
| CE a)  | Se han analizado las limitaciones que plantea la ejecución de aplicaciones en dispositivos móviles. |
| CE b)  | Se han identificado las tecnologías de desarrollo de aplicaciones para dispositivos móviles. |
| CE c)  | Se han instalado, configurado y utilizado entornos de trabajo para el desarrollo de aplicaciones para dispositivos móviles. |
| CE d)  | Se han identificado configuraciones que clasifican los dispositivos móviles en base a sus características. |
| CE e)  | Se han descrito perfiles que establecen la relación entre dispositivos y tipos de aplicaciones. |
| CE f)  | Se han reconocido las limitaciones técnicas de los dispositivos móviles. |
| CE g)  | Se han analizado las capacidades gráficas y sonoras de los dispositivos en base a las necesidades de la aplicación. |
| CE h)  | Se han reconocido los mecanismos de seguridad y distribución de software específicos de dispositivos móviles. |

!!! info "Qué deberías saber al terminar"
    Al acabar este tema deberías poder:

    - describir los tipos de dispositivos móviles y sus limitaciones para el desarrollo;
    - repasar la evolución de las redes de comunicaciones móviles y su capacidad actual;
    - comparar el desarrollo nativo con el multiplataforma y elegir según el caso;
    - instalar Android Studio y crear un primer proyecto de aplicación;
    - configurar emuladores (AVD) y justificar cuándo es necesario un dispositivo real;
    - explicar la estructura de un proyecto Android y el ciclo de vida de una aplicación.

!!! tip "Mapa del tema"
    En este documento vamos a seguir esta secuencia:

    1. dispositivos móviles: evolución, tipos y características;
    2. redes de comunicaciones móviles: de la 0G a la 6G;
    3. sistemas operativos móviles y tecnologías de desarrollo;
    4. emuladores: configuraciones, perfiles y dispositivos soportados;
    5. aplicaciones móviles: estructura, jerarquía de clases y ciclo de vida;
    6. entorno de ejecución del administrador de aplicaciones.

### 1. Dispositivos móviles: evolución, tipos y características

Un **dispositivo móvil** es un aparato de pequeño tamaño, portable, con autonomía de funcionamiento suministrada por una batería, conexión permanente o semipermanente a una red de comunicaciones y con varias funciones. La función original de los dispositivos móviles era llamar por teléfono; hoy un smartphone puede hacer muchas funciones que antes realizaban aparatos y sistemas diferentes.

!!! definition "Definición"
    <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Dispositivo móvil: aparato de pequeño tamaño, portable, con autonomía de funcionamiento suministrada por una batería, conexión permanente o semipermanente a una red de comunicaciones y con varias funciones.</span>

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Es importante conocer las características de los dispositivos para los cuales vamos a diseñar y desarrollar las aplicaciones, porque sus limitaciones condicionan el diseño.</span>

**Características principales:**

| Característica | Qué implica |
|----------------|--------------|
| Tamaño | Para ser portable debe tener un tamaño reducido, pudiendo portarse incluso durante actividades deportivas. |
| Movilidad | Por su tamaño y no dependencia de cableado: tamaño, batería duradera y comunicación inalámbrica. |
| Conectividad | Uso de comunicaciones inalámnicas; las redes han evolucionado mucho y han permitido la incorporación masiva de estos dispositivos. |
| Capacidad de procesado | Requieren microprocesadores capaces de realizar los procesos de cálculo y de gestionar la información a la que acceden. |

**Características secundarias:** sistema operativo, ergonomía, diseño, tamaño de la pantalla y gadgets compatibles.

**Tipos de dispositivos móviles:**

- **Smartphones.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Con una función principal de teléfono móvil, pero por sus características se parece a un ordenador personal: pantalla de alta calidad, múltiple conectividad, gran capacidad de procesamiento y almacenamiento y dispositivos incorporados (cámara, sensores, GPS).</span>
- **Tablets.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Con tamaño intermedio entre el ordenador y el móvil. Son ligeros, autónomos y con fácil manejo (pantalla táctil).</span>
- **PDAs** (Personal Digital Assistant). Dispositivos que intentan combinar prestaciones para servir de organizador personal. Su uso está en declive.
- **eBooks.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Libro electrónico. Ligeros, con gran capacidad de almacenaje y tinta electrónica.</span>
- **Wearables.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El más difundido es el smartwatch o reloj inteligente.</span>

<figure markdown="span">
  ![Tipos de dispositivos móviles](assets/tipos-dispositivos-moviles.png)
  <figcaption>Tipos de dispositivos móviles: la familia no se limita al smartphone.</figcaption>
</figure>

!!! example "Ejemplo"
    Una misma aplicación de mensajería debe funcionar en un móvil de 6 pulgadas, una tablet de 10 y un reloj: misma lógica, tres interfaces radicalmente distintas.

**Limitaciones para el desarrollo:**

- La mayoría de las limitaciones están relacionadas con el **hardware**, que puede limitar la capacidad de procesamiento y almacenaje, aunque se ha mejorado mucho.
- **Diversidad en el tamaño de la pantalla**: <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">debemos diseñar con estilo responsive o tener en cuenta las variaciones.</span>
- No todos los teléfonos tendrán los mismos **sensores y GPS**.
- No todas las herramientas y librerías de terceros son soportadas por todos los dispositivos.
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Antes de lanzar una aplicación a explotación se debe **probar en varios dispositivos reales o emuladores**.</span>

!!! note "Aclaración"
    Hoy hay más de 5 millones de aplicaciones repartidas entre los grandes repositorios (Play Store, App Store y otras). Las descargas anuales superaban los 40 billones en 2020 (15% juegos) y los 250 billones en 2024 (un 34% juegos): el mercado justifica el esfuerzo de conocerlo bien.

<figure markdown="span">
  ![Tiendas de aplicaciones](assets/tiendas-apps.png)
  <figcaption>Los grandes repositorios de aplicaciones: App Store y Google Play.</figcaption>
</figure>

### 2. Redes de comunicaciones móviles

Las redes de comunicaciones móviles han ido evolucionando con el tiempo. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Actualmente se está implantando la quinta generación (5G), pero ya se investiga la 6G.</span>

| Generación | Época | Tecnología | Velocidad típica | Hitos |
|------------|-------|------------|------------------|-------|
| 0G | Años 30-40 | Onda de radio (PTT, IMTS) | Solo voz | Walkie talkies; difusión en la II Guerra Mundial |
| 1G | 1977 | AMPS (EE. UU.), NMT (Escandinavia) | Solo voz, analógica | Primera red celular en Chicago; equipos grandes y pesados, baja seguridad |
| 2G | Años 90 | GSM (Europa), IS-136 (EE. UU.) | Voz + datos digitales | <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Tarjeta SIM, SMS; tecnología digital</span> |
| 2.5G/2.75G | Finales 90 | GPRS, EDGE | Navegación básica | Redes de respaldo aún vigentes |
| 3G | 2001 (Japón) | UMTS, CDMA | <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Hasta 2 Mbps</span> | Convergencia voz/datos; navegación fluida; auge de BlackBerry |
| 3.5G/3.75G | ~2005 | HSPA | Hasta 14 Mbps | Mejora de la 3G en velocidad |
| 4G | 2010 | LTE, UMTS, WiMax | 60 Mbps bajada / 40 subida | Bandas 800/1800/2600 MHz; señal de televisión |
| 4G+ | 2016 | LTE-A | 300 Mbps / 50 Mbps | Banda de 700 MHz |
| 5G | 2017 (MWC Barcelona) | NR | <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">1-10 Gbps</span> | Antenas mejoradas; IoT, WSN, AR/VR comunicada |
| 6G | Investigación | Satélites | Hasta 100 Gbps | Cobertura global; frecuencias más elevadas |

!!! example "Ejemplo"
    Una app de vídeo en streaming necesita el ancho de banda de una 4G para no tartamudear; un chat funciona incluso en 2G de respaldo. La generación de red disponible condiciona qué puede hacer tu aplicación y cómo debe degradarse con elegancia cuando la red empeora.

<figure markdown="span">
  ![Red 5G sobre una ciudad](assets/red-5g.jpg)
  <figcaption>La 5G integra redes celulares, sensores inalámbricos (WSN) e IoT, y supera el ámbito del teléfono.</figcaption>
</figure>

!!! warning "Atención"
    El contexto de red cambia constantemente (red, luz, batería, orientación), así que <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">la app debe ser **responsiva, eficiente y resiliente**: capaz de pausar y reanudar sin perder estado.</span>

### 3. Sistemas operativos móviles

Los sistemas operativos (OS) móviles, al igual que en los ordenadores personales, <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">gestionan el conjunto de programas de bajo nivel que permiten la abstracción del hardware concreto y proporcionan servicios a las aplicaciones que se ejecutan sobre él</span>. La mayoría de los SO móviles se organizan en un **sistema de capas**:

1. **Kernel o núcleo.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Permite el acceso a los elementos del hardware, ofreciendo servicios a las capas superiores a través de controladores o drivers, como la gestión de procesos, gestión de la memoria o el sistema de archivos.</span>
2. **Middleware.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Conjunto de módulos que permite la existencia de aplicaciones. Es transparente al usuario.</span>
3. **Gestor de aplicaciones.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Aplicaciones nativas o multiplataforma.</span>

#### Android

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Es el sistema operativo líder en el mercado. Fue lanzado por la Open Handset Alliance (OHA) en 2007, a través de una agrupación de 78 compañías lideradas por Google.</span> Sus capas, de abajo arriba:

1. **Kernel de Linux.**
2. **Capa de abstracción de hardware (HAL).**
3. **Runtime.** Cada app ejecuta sus propios procesos con sus propias instancias de tiempo de ejecución.
4. **Biblioteca C/C++ nativas.** Muchos componentes y servicios están programados en estos lenguajes.
5. **Framework de API Java.** Acceso a funciones de Android a través de una API.
6. **Apps del sistema.** Conjunto de apps nativas para correo electrónico, navegador, calendario...

<figure markdown="span">
  ![Arquitectura por capas de Android](assets/arquitectura-android.png)
  <figcaption>La pila de software de Android: del kernel de Linux a las apps del sistema.</figcaption>
</figure>

#### iOS

Fue desarrollado por Apple, se llamó originalmente iPhone OS y es un derivado de Mac OS X. Su principal punto fuerte es la combinación excelente entre hardware y software, así como el uso de pantalla multitáctil desde sus comienzos. Solo funciona en los dispositivos de la propia empresa (iPhone, iPad), lo que le permite una integración total entre hardware y software y actualizaciones simultáneas para toda su gama. Sus aplicaciones se desarrollan en **Swift** (o el veterano Objective-C) con Xcode, y se distribuyen exclusivamente a través de la App Store, cuyo proceso de revisión es más estricto que el de otras tiendas.

#### Otros sistemas operativos móviles

- **Windows Phone.** Desarrollado por Microsoft y lanzado en 2010.
- **BlackBerry OS.**
- **Symbian.**
- **Firefox OS.** Desarrollado por Mozilla, con núcleo Linux y basado en HTML5, pensado en dispositivos de gama baja.
- **Ubuntu Touch.**
- **HarmonyOS.** Desarrollado por Huawei Technologies, diseñado además para la intercomunicación de dispositivos IoT.

### 4. Tecnologías de desarrollo: nativas y multiplataforma

Para la programación de dispositivos móviles existen una gran cantidad de herramientas, entornos y lenguajes. En primer lugar hay que decidir qué lenguaje se va a usar y, por otra parte, si se opta por desarrollo nativo o multiplataforma.

#### Desarrollo nativo

Supone emplear las herramientas y lenguajes propios de cada sistema operativo:

- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Sus principales ventajas: apps muy optimizadas, máximo rendimiento y mayor flexibilidad para el programador.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Su principal desventaja: si se quiere desarrollar para otras plataformas, el tiempo de desarrollo es mayor.</span>

| Plataforma | Lenguajes |
|------------|-----------|
| Android | Java, Kotlin |
| iOS | Objective-C, Swift, Cocoa Touch |
| Windows Phone | C# + XAML |

#### Desarrollo multiplataforma

Distinguimos dos tipos:

- **Compilado a nativo.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Usa una plataforma intermedia, como **Xamarin**, que con un único lenguaje (C# sobre .NET) puede programar para varias plataformas. Las aplicaciones se compilan después en código nativo, lo que le da el mismo rendimiento que el desarrollo nativo.</span>
- **Basado en HTML5.** <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Utilización de herramientas basadas en HTML5 que generan aplicaciones para todas las plataformas, como **PhoneGap/Cordova** o **Ionic**. No tienen el mismo rendimiento que las aplicaciones nativas.</span>

#### Lenguajes y entornos destacados

**Kotlin.** Fue desarrollado por JetBrains en 2016. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Dentro del desarrollo nativo Android está tomando gran fuerza por encima de Java, al ser señalado por Google como lenguaje principal. Corre sobre la máquina virtual de Java.</span> Sus ventajas: simplicidad, más seguro que Java, robusto y orientado a objetos, con acceso a gran número de librerías, sintaxis más limpia (necesita menos código) y coexistencia simultánea con Java.

**Java.** Es el lenguaje nativo original para Android. Paralelo a Java es necesario conocer bien XML para el desarrollo de aplicaciones. Herramienta de desarrollo: Android Studio, un IDE basado en IntelliJ IDEA que se lanzó para reemplazar a Eclipse.

**Ionic.** Permite desarrollar y desplegar aplicaciones híbridas que funcionan en múltiples plataformas (iOS, Android, escritorio y web como PWA) con una única base de código. Ofrece un diseño limpio, sencillo y funcional. Está construido sobre tecnologías web (HTML, CSS y JavaScript) y se puede usar con los frameworks frontend más populares: Angular, React y Vue.

**Flutter.** SDK de Google para crear aplicaciones Android e iOS, para móvil, web y escritorio, desde una única base de código. Su lenguaje es <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Dart**</span> y se caracteriza por el desarrollo rápido gracias a <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Hot Reload**: se pueden hacer cambios sin parar y arrancar la aplicación, reduciendo las esperas.</span>

**Xamarin.** Entorno de desarrollo creado por Microsoft que permite crear aplicaciones Android e iOS en C#. Es gratuito y va integrado en Visual Studio. Pese a no desarrollar para Android en Java, permite crear una aplicación nativa que asegura una buena experiencia de usuario.

**NativeScript.** Entorno de desarrollo de código abierto para crear aplicaciones móviles Android e iOS. Lenguajes: Angular, Vue.js, TypeScript o JavaScript. Su principal ventaja es el rendimiento nativo: defines una vez y se adapta para ejecutarse en todas partes, adaptando la interfaz a dispositivos y pantallas concretos.

!!! example "Ejemplo"
    Para una app corporativa interna que deba salir ya para Android e iOS con un equipo pequeño, Flutter o Xamarin reducen a la mitad el esfuerzo. Para una app de realidad aumentada con máximo rendimiento gráfico, el camino razonable es nativo: Kotlin en Android, Swift en iOS.

!!! note "Aclaración"
    Unity3D es un ejemplo de entorno **específico para videojuegos**, frente a entornos de carácter general como Android Studio. En este módulo nos centraremos en el entorno Android Studio + Kotlin.

#### Instalación de Android Studio

- Descarga desde <https://developer.android.com>.
- Android Studio está basado en IntelliJ (IDE para Java, como Eclipse o Netbeans).
- Dentro de Android Studio se puede programar con Kotlin.
- No solo se pueden programar apps para móviles, sino también para wearables, TV smart y otros.

Los pasos del instalador (dejar las opciones por defecto) llevan hasta la pantalla de bienvenida, desde donde se crea el primer proyecto. En la siguiente captura se ve un proyecto recién creado con plantilla *Empty Activity*, mientras Gradle sincroniza las dependencias:

<figure markdown="span">
  ![Primer proyecto en Android Studio](assets/android-studio-primer-proyecto.png)
  <figcaption>Primer proyecto en Android Studio: estructura del proyecto y MainActivity.kt en Kotlin con plantilla Jetpack Compose.</figcaption>
</figure>

### 5. Emuladores: configuraciones, perfiles y dispositivos soportados

Para qué sirven: <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">los emuladores y simuladores permiten ejecutar y depurar apps **sin dispositivo físico**, variando tamaño de pantalla, versión del SO, sensores y condiciones de red.</span>

**Configuración clave de un AVD (Android Virtual Device):**

| Aspecto | Opciones |
|---------|----------|
| API/Imagen del sistema | Elige la versión (nivel de API) que quieres probar |
| ABI | x86_64/arm64 (x86_64 suele ser más rápido en PC) |
| Hardware | RAM, almacenamiento, cámara virtual (frontal/trasera), aceleración (Hyper-V/WHPX en Windows, HAXM obsoleto; KVM en Linux; nativo en macOS) |
| Pantalla | Tamaño (pulgadas), densidad (dpi), rotación, "safe areas" (notch, recortes) |
| Sensores | GPS (inyectar lat/lon), acelerómetro/giroscopio (controles en AVD), batería (nivel/temperatura) |
| Red | Perfiles de latencia/pérdidas (3G/4G/5G/poor) |

**Perfiles útiles:**

- Gama baja: 2 GB RAM, pantalla 5", dpi alto, API n−2.
- Gama media: 3-4 GB RAM, 6-6.5", API actual.
- Pantalla grande/Tablet: 10-12", comprobar layouts adaptativos.
- Foldable: probar continuity al plegar/desplegar.
- Wear OS/TV/Auto: emuladores específicos para relojes/TV/coche.

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Cuándo usar dispositivo real:** cámara avanzada, BLE/NFC, rendimiento fino, gestos complejos y casos OEM.</span>

<figure markdown="span">
  ![Emulador AVD en ejecución junto al editor de layouts](assets/emulador-avd.png)
  <figcaption>Emulador Android (AVD) en ejecución junto al editor visual de interfaces de Android Studio.</figcaption>
</figure>

### 6. Aplicaciones móviles: estructura y jerarquía de clases

Los proyectos que se crean en Android Studio tienen una estructura bien definida. Veamos **para qué sirve cada carpeta** del árbol:

```text
MiProyecto/
 ├─ app/                          ← el MÓDULO de tu aplicación (lo que se compila e instala)
 │   ├─ src/main/                 ← el código FUENTE de la app
 │   │   ├─ AndroidManifest.xml   ← el DNI de la app: nombre, icono, activities, permisos
 │   │   ├─ java/ (o kotlin/)     ← TU CÓDIGO: activities, viewmodels, repositorios...
 │   │   └─ res/                  ← RECURSOS (todo lo que no es código)
 │   │        ├─ layout/          ← interfaces en XML (si no usas Compose)
 │   │        ├─ values/          ← constantes: strings.xml (textos), colors.xml, themes
 │   │        ├─ drawable/        ← imágenes e iconos
 │   │        ├─ mipmap/          ← iconos de la app en varios tamaños
 │   │        └─ xml/             ← configuraciones varias (permisos, backups...)
 │   └─ build.gradle(.kts)        ← receta del módulo: dependencias, versión del SDK...
 ├─ build.gradle(.kts)            ← receta del PROYECTO (plugins, repositorios)
 ├─ settings.gradle(.kts)         ← qué módulos forman el proyecto
 └─ gradle.properties             ← opciones de Gradle (memoria, AndroidX...)
```

**¿Para qué vale cada cosa?**

| Elemento | Para qué sirve | Ejemplo |
|----------|----------------|---------|
| `AndroidManifest.xml` | <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Declara la identidad de app: paquete, nombre, icono, qué activities contiene y qué permisos necesita. Sin manifest, Android no sabe que tu app existe.</span> | `<uses-permission android:name="android.permission.CAMERA"/>` declara que usarás la cámara |
| `java/` o `kotlin/` | Todo tu código Kotlin: la lógica, las pantallas, el acceso a datos. Es lo único que "programas" en sentido estricto. | `MainActivity.kt` |
| `res/layout/` | Interfaces definidas en XML (la forma clásica, pre-Compose). Cada archivo es una pantalla o fragmento de pantalla. | `activity_main.xml` |
| `res/values/strings.xml` | Los textos de la app separados del código: así se puede traducir la app cambiando un solo archivo. | `res/values-es/strings.xml` = versión española |
| `res/drawable/` | Imágenes, iconos y formas dibujables que usa la interfaz. | `logo.png` |
| `res/mipmap/` | El icono del launcher en distintas densidades (hdpi, xhdpi...) para que se vea nítido en cualquier pantalla. | `ic_launcher` |
| `build.gradle(.kts)` | La "receta de construcción": qué librerías descarga (dependencias), versión mínima de Android soportada, versión con la que se compila. | `implementation("androidx.compose.material3:material3")` |
| `settings.gradle(.kts)` | Lista los módulos que componen el proyecto. | `include(":app")` |

**Componentes software de una aplicación. Estructura por capas:**

⚠️ **Antes de nada:** la numeración de esta lista es **orden de lista, NO un diagrama de capas**. El 1 no está "abajo" y el 8 "arriba": si los colocas como pila real, el orden cambia y hay componentes que ni siquiera tienen piso propio. El diagrama de capas correcto está justo debajo de la tabla.

| # | Componente | 📍 Su sitio real | Ejemplo real que puedes probar hoy |
|---|------------|:--:|------------------------------------|
| 1 | **Activity** — pantalla con la que interactúas; **punto de entrada** de la app; reacciona a eventos (tocs, girar el móvil...). Una app típica tiene varias | 🟩 Arriba (caparazón de la UI) | **WhatsApp**: la lista de chats es una; tocas un chat y se arranca la actividad de la conversación; botón atrás y vuelves. **Gmail**: bandeja → tocar un correo abre la actividad del mensaje |
| 2 | **Fragment** — "trozo de pantalla" reutilizable que vive DENTRO de una Activity | 🟩 Arriba (dentro de la Activity) | **Instagram**: la pantalla principal es UNA actividad y las pestañas de abajo (Inicio / Buscar / Reels) son fragments que se intercambian sin cambiar de actividad. **Gmail en tablet**: lista a la izquierda + mensaje a la derecha = dos fragments en la misma actividad; en móvil, el mismo código muestra uno solo |
| 3 | **ViewModel** — guarda el **estado** de la pantalla y su lógica de presentación; **sobrevive a rotaciones**: al girar, la Activity se destruye y recrea, el ViewModel conserva los datos | 🟦 Segundo piso | Prueba en directo: escribe un comentario largo en **X/Twitter** y gira el móvil → el texto NO se pierde y la lista NO se recarga. La actividad se destruyó delante de tus narices; el ViewModel siguió vivo |
| 4 | **Repository** — única puerta de entrada y salida de **datos**: decide si lee de red (API REST), de base de datos (SQLite/Room) o de caché. Las capas superiores no saben de dónde vienen los datos | 🟨 Tercer piso | **Just Eat / Uber Eats**: el catálogo llega por API REST, tus favoritos viven en Room y sin cobertura ves el último catálogo desde caché. Los tres caminos detrás de UNA puerta. **Spotify**: playlists cacheadas que se sincronizan con red; la UI no sabe si pinta datos de ayer o de hoy |
| 5 | **Service** — trabajo en **segundo plano** sin interfaz | 🟪 Lateral (no es piso) | **Spotify**: dale al play, cierra la app DE DAVO y la música sigue → foreground service con notificación persistente. **Play Store**: actualiza 20 apps con la tienda cerrada |
| 6 | **BroadcastReceiver** — "receptor de radio" que escucha eventos del sistema (batería baja, WiFi, arranque) y reacciona | 🟪 Lateral (no es piso) | Música sonando y **desenchufas los auriculares** → se pausa solo (`ACTION_AUDIO_BECOMING_NOISY`). **WhatsApp**: al volver la cobertura se reconecta solo y entran los pendientes |
| 7 | **ContentProvider** — comparte tus datos con OTRAS apps de forma controlada | 🟪 Lateral (no es piso) | Cuando **WhatsApp pide tus contactos**, lee a través del ContactsProvider de la app nativa. El **selector de fotos** de Instagram es el MediaStore: te cede solo la foto que eliges, no la galería entera |
| 8 | **UI** — tradicional (XML) o **Jetpack Compose** (UI declarativa en Kotlin) | 🟩 Lo más de arriba: lo que ve el usuario | XML clásico: los ajustes de cualquier app bancaria española. Compose: *Now in Android* (Google, 100 % Compose), **Netflix** (migró su UI a Compose) y **Spotify**, que en 2024 usa Compose Multiplatform: el mismo código de UI en Android e iOS |

Y ahora el diagrama de capas **de verdad**, de lo que ve el usuario hacia abajo:

```mermaid
flowchart TD
    USR[👁 El usuario] --> V
    subgraph PILA["LA PILA — de arriba abajo"]
        V["🟩 UI (XML / Compose)<br/>hostada en Activity y Fragment"]
        VM["🟦 ViewModel<br/>estado + lógica de pantalla"]
        R["🟨 Repository<br/>la puerta de los datos"]
        D["⬛ Fuentes: API REST · Room/SQLite · caché"]
        V --> VM --> R --> D
    end
    subgraph LADO["🟪 PUERTAS LATERALES — no son pisos"]
        S["Service<br/>segundo plano"]
        B["BroadcastReceiver<br/>escucha eventos"]
        C["ContentProvider<br/>comparte datos"]
    end
    S -.->|alimenta| R
    B -.->|despierta| VM
    C -.->|exponen el repo al exterior| R
    style V fill:#2f855a,color:#fff
    style VM fill:#2b6cb0,color:#fff
    style R fill:#d69e2e,color:#000
    style D fill:#4a5568,color:#fff
    style S fill:#805ad5,color:#fff
    style B fill:#805ad5,color:#fff
    style C fill:#805ad5,color:#fff
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">El apilamiento vertical real es **UI → ViewModel → Repository → fuentes de datos**, y esa tríada es la que pregunta el tribunal cuando dice "estructura por capas".</span> <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">Activity y Fragment alojan la UI (viven arriba); Service, BroadcastReceiver y ContentProvider son **puertas laterales al sistema**, no estratos: un Service puede alimentar al Repository, pero no "vive debajo de él".</span>

!!! danger "Trampa de examen"
    Si te preguntan por el orden de capas, **no contestes "1 a 8"** ni "Activity abajo y Compose arriba": eso es el orden de la lista del temario, no la arquitectura. La respuesta: pantalla arriba (UI en Activity/Fragment), debajo el ViewModel, debajo el Repository, y al fondo las fuentes de datos (red, base de datos, caché). Los componentes de sistema (Service, Receiver, Provider) van en un lateral: entran y salen de la pila según el caso.


**Jerarquía típica con patrón MVVM:**

Antes de ver la cadena, definamos las siglas: **MVVM** = *Model-View-ViewModel*, un patrón que separa la interfaz (View) del estado y lógica de pantalla (ViewModel) y de los datos (Model). El objetivo: que cada capa se pueda cambiar y probar sin romper las otras.

La analogía que lo deja claro en diez segundos: **un restaurante**. La **View es el comedor**: lo único que el cliente ve (mesa, carta, presentación del plato). El **ViewModel es el camarero**: apunta lo que pide el cliente (evento que SUBE) y trae lo que se cocina (estado que BAJA); no cocina y no se sienta con el cliente. El **Model es la cocina**: género, fogones y despensa (datos: API, base de datos, ficheros); el cliente nunca entra en la cocina, y si mañana cambias los fogones, el comedor ni se entera. Regla única del patrón: **los eventos suben (clics), el estado baja (datos para pintar), y nadie habla con un vecino que no tenga al lado.**

Vamos a verlo con tres ejemplos pequeños y distintos. En todos: 🟩 = View (lo que se ve), 🟦 = ViewModel (estado + lógica), 🟨/⬛ = Model (datos).

#### Ejemplo 1 — La app del tiempo (el Model habla con una API)

```mermaid
flowchart LR
    A["⬛ API del tiempo<br/>json: {t:21,d:'sol'}"] --> R["🟨 MeteoroRepository<br/>única puerta de datos"]
    R --> VM["🟦 MeteoroViewModel<br/>estado: Meteoro?"]
    VM --> V["🟩 ClimaCard<br/>🌤 Cádiz · 21°"]
    style A fill:#4a5568,color:#fff
    style R fill:#d69e2e,color:#000
    style VM fill:#2b6cb0,color:#fff
    style V fill:#2f855a,color:#fff
```

```kotlin
// ── MODELO (la cocina) ─────────────────────────────────────────
data class Meteoro(val ciudad: String, val grados: Int, val icono: String)  // [M] dato puro

class MeteoroRepository {                       // [M] única puerta de entrada
    suspend fun getMeteoro(c: String): Meteoro =
        apiRest.getMeteoro(c)                   // [M] hoy llama a red; mañana a caché
}                                               // [M] y la View ni se entera del cambio

// ── VIEWMODEL (el camarero) ────────────────────────────────────
class MeteoroViewModel : ViewModel() {          // [VM] cerebro de esta pantalla
    private val repo = MeteoroRepository()      // [VM] solo habla con la cocina
    private val _meteoro = MutableStateFlow<Meteoro?>(null)     // [VM] el estado
    val meteoro: StateFlow<Meteoro?> = _meteoro.asStateFlow()   // [VM] lo que la View observa

    init {                                      // [VM] al nacer la pantalla…
        viewModelScope.launch {                 // [VM] …pedimos el dato a la cocina
            _meteoro.value = repo.getMeteoro("Cádiz")           // [VM] y lo guardamos
        }
    }
}

// ── VISTA (el comedor) ─────────────────────────────────────────
@Composable
fun ClimaCard(vm: MeteoroViewModel) {           // [V] la pantalla
    val m by vm.meteoro.collectAsState()        // [V] OBSERVA el estado
    m?.let { Text("${it.icono} ${it.ciudad} · ${it.grados}°") } // [V] y lo PINTA
}
```

> **Veredicto:** el **Model** es `Meteoro` + `MeteoroRepository` (sabe de JSON y de red, no sabe que existe `Text`). El **ViewModel** es `MeteoroViewModel` (guarda el `Meteoro?` y lo carga al nacer). La **View** es `ClimaCard` (no sabe que hay una API: solo ve un dato nullable y lo pinta cuando llega).

#### Ejemplo 2 — El contador que sobrevive a reinicios (el Model guarda en disco)

```mermaid
flowchart LR
    V["🟩 Button +1 y Text<br/>'3 pulsaciones'"] -.->|"evento SUBE"| VM["🟦 ContadorViewModel<br/>incrementar()"]
    VM -->|"guardar"| M["⬛ PrefsStore<br/>SharedPreferences"]
    VM -.->|"estado BAJA"| V
    style V fill:#2f855a,color:#fff
    style VM fill:#2b6cb0,color:#fff
    style M fill:#4a5568,color:#fff
```

```kotlin
// ── MODELO: el número vive aquí, no en la pantalla ─────────────
object PrefsStore {                             // [M] la despensa: persistencia
    private var guardado = 0                    // [M] en la vida real: SharedPreferences
    fun leer(): Int = guardado                  // [M]
    fun escribir(n: Int) { guardado = n }       // [M]
}

// ── VIEWMODEL: la única regla de negocio de la pantalla ────────
class ContadorViewModel : ViewModel() {         // [VM]
    private val _cuenta = MutableStateFlow(PrefsStore.leer())   // [VM] arranca con lo guardado
    val cuenta: StateFlow<Int> = _cuenta.asStateFlow()          // [VM] lo que se observa

    fun incrementar() {                         // [VM] el camarero apunta el pedido
        _cuenta.value += 1                      // [VM] actualiza el estado → la pantalla se redibuja
        PrefsStore.escribir(_cuenta.value)      // [VM] y avisa a la cocina: guarda en disco
    }
}

// ── VISTA: pinta y reenvía, sin decidir nada ────────────────────
@Composable
fun Contador(vm: ContadorViewModel) {           // [V]
    val n by vm.cuenta.collectAsState()         // [V] observa
    Column {
        Text("Vas por $n")                      // [V] pinta el estado que BAJA
        Button(onClick = { vm.incrementar() }) { Text("+1") }  // [V] el clic SUBE al VM
    }
}
```

> **Veredicto:** el **Model** es `PrefsStore` (el número VIVE ahí; por eso sobrevive a reinicios). El **ViewModel** es `ContadorViewModel` (la única regla: +1, avisar a la View y pedir guarda). La **View** es `Contador` (no sabe si el número está en memoria, en disco o en Marte). Si mañana el contador se guarda en una API, solo tocas el Model: `vm.incrementar()` y el `Text` no cambian ni una letra.

#### Ejemplo 3 — La lista de la compra compartida: la cadena completa

Cada pieza de la cadena:

- **MainActivity** (View): la pantalla. Su único trabajo es **dibujar** lo que le dice el ViewModel y **reenviarle** los clics del usuario. No consulta datos ni calcula nada.
- **HomeFragment** (View): ídem, un fragmento de esa pantalla.
- **HomeViewModel** (ViewModel): el "cerebro de la pantalla". Guarda el estado (la lista de items que se está viendo) y la lógica (qué pasa al pulsar "borrar"). Si la pantalla rota, él sobrevive.
- **ItemsRepository** (Model/Repository): el "bibliotecario de datos". Le piden items y él decide si ir a la API REST o a SQLite. Único punto de acceso a datos.
- **API REST / SQLite**: la fuente real de los datos: un servidor en internet o una base de datos local.
- **Clases de dominio** (p. ej. `Item`): los "contenedores de datos" puros (data classes) que viajan por toda la cadena: `data class Item(val id: Int, val titulo: String)`.

**La traza completa, paso a paso** — del arranque al dato en pantalla:

```text
[1] El usuario toca el icono de la app
        │
        ▼
[2] Android lee AndroidManifest.xml y arranca MainActivity (la activity "launcher")
        │
        ▼
[3] MainActivity se crea y pide su ViewModel al sistema:
    val vm: HomeViewModel by viewModels()
        │
        ▼
[4] HomeViewModel al nacer llama al repositorio:
    init { cargarItems() }
        │
        ▼
[5] ItemsRepository decide la fuente y trae los datos:
    suspend fun getItems(): List<Item> = apiRest.getItems()   // o Room (SQLite)
        │
        ▼
[6] El ViewModel recibe la List<Item> y la guarda en su estado:
    private val _items = MutableStateFlow<List<Item>>(emptyList())
    val items: StateFlow<List<Item>> = _items
        │
        ▼
[7] MainActivity (la View) observa ese estado y se redibuja al cambiar:
    collectAsState() → cuando llegan los items, Compose los pinta en pantalla
        │
        ▼
[8] El usuario VE la lista. Si toca un item → el clic sube al ViewModel (paso 3 al revés)
```

Y el código real de cada eslabón (simplificado):

```kotlin
// ── [Modelo] La clase de dominio: el contenedor de datos ──
data class Item(val id: Int, val titulo: String)

// ── [Model] El repositorio: única puerta a los datos ──
class ItemsRepository {
    suspend fun getItems(): List<Item> = listOf(
        Item(1, "Comprar leche"), Item(2, "Estudiar PMDM")
    )   // aquí iría la llamada real: apiRest.getItems() o roomDao.getAll()
}

// ── [ViewModel] El cerebro de la pantalla: estado + lógica ──
class HomeViewModel : ViewModel() {
    private val repo = ItemsRepository()

    private val _items = MutableStateFlow<List<Item>>(emptyList())
    val items: StateFlow<List<Item>> = _items.asStateFlow()   // lo que observa la View

    init { cargarItems() }                                    // al nacer, carga

    fun cargarItems() {
        viewModelScope.launch {
            _items.value = repo.getItems()                    // pide datos y actualiza estado
        }
    }
}

// ── [View] La pantalla: solo dibuja y reenvía clics ──
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val vm: HomeViewModel by viewModels()
        setContent {
            val items by vm.items.collectAsState()   // observa el estado
            LazyColumn {                               // y lo pinta
                items(items) { item -> Text(item.titulo) }
            }
        }
    }
}
```

!!! note "Aclaración"
    En Jetpack Compose la interfaz ya no se describe en XML: se construye con funciones `@Composable` en Kotlin. La plantilla *Empty Activity* actual ya genera código Compose, como se vio en la captura de Android Studio.

> **Veredicto del ejemplo 3:** el **Model** son `Item`, `ItemsRepository` y la fuente (API o SQLite). El **ViewModel** es `HomeViewModel`: su `_items: MutableStateFlow<List<Item>>` es el único sitio donde vive "lo que hay en pantalla". La **View** es `MainActivity`: `collectAsState()` observa y `LazyColumn` pinta; ni una consulta a datos dentro. Misma receta que los ejemplos 1 y 2, solo que con lista.

### 7. Modelo de estados y ciclo de vida de una app

**Estados: activo, pausado y destruido.**

1. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Activo (foreground):** la app está visible e interactiva.</span>
2. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Pausa/Background:** perdió foco (otra app delante o pantalla bloqueada). Debe guardar estado, parar sensores/tareas y reducir consumo.</span>
3. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Destruido/Terminado:** el sistema o el usuario la cierra; los recursos se liberan.</span>

**¿Y cómo sé EN qué estado está mi app?** Android no te lo "dice" directamente: te **avisa llamando a métodos** de tu Activity en cada transición. Son los **callbacks del ciclo de vida**, y TÚ decides qué hacer en cada uno sobrescribiéndolos. Si no los sobrescribes, existen igual (con cuerpo vacío) — igual que pasaba con los parámetros por defecto de `Row`: no son obligatorios, pero ahí están.

El mapa completo de callbacks:

```text
                    ┌─────────────┐
                    │  onCreate() │  ← la app NACE: crear interfaz, cargar lo mínimo
                    └──────┬──────┘
                           ▼
                    ┌─────────────┐
              ┌─────│  onStart()  │◄────────────────┐
              │     └──────┬──────┘  visible        │
              │            ▼                        │
              │     ┌─────────────┐         ┌───────┴───────┐
              │     │ onResume()  │         │    onRestart() │
              │     │ INTERACTIVA │         │ (volver de     │
              │     └──────┬──────┘         │  onStop)       │
              │            │                └───────┬───────┘
              │            ▼                        │
              │     ┌─────────────┐                 │
              │     │  onPause()  │  pierde foco    │
              │     └──────┬──────┘                 │
              │            ▼                        │
              │     ┌─────────────┐                 │
              └────►│  onStop()   │─────────────────┘
                    │  invisible  │   (vuelve a ser visible → onRestart)
                    └──────┬──────┘
                           ▼
                    ┌──────────────┐
                    │ onDestroy()  │  la app MUERE: liberar todo
                    └──────────────┘
```

**Cómo leerlo (reglas mnemotécnicas):**

- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`onCreate → onStart → onResume` es la **secuencia de arranque**. Se ejecutan SIEMPRE las tres, en ese orden.</span>
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`onResume` no significa "reanudar después de una pausa": significa **"la app está ahora interactiva"**.</span> Es el último en ejecutarse al arrancar.
- **¿La app está corriendo/activa?** → cuando se ha ejecutado `onResume()` y aún no ha llegado `onPause()`.
- **¿Cómo lo veo yo, el programador?** → con **`Log.d()` + Logcat**. Es el "printf de Android": pintas un mensaje en cada callback y los ves aparecer en la pestaña *Logcat* de Android Studio, con la app real ejecutándose. El código exacto, más abajo.
- `onPause` = "pierdo el foco" (半 visible: un diálogo encima, va a cerrarse). `onStop` = "ya no se ve nada". No son lo mismo: en un diálogo solo llega `onPause`.
- <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">`onDestroy` **no está garantizado**: si el sistema mata el proceso desde `onStop`, tu app desaparece sin pasar por él. Por eso lo importante se guarda ANTES (en `onPause`/`onStop`).</span>

**El mapeo estados ↔ callbacks:**

| Estado de la app | Callbacks que se han ejecutado | Qué debería hacer la app |
|------------------|-------------------------------|--------------------------|
| **Activa** (visible e interactiva) | `onCreate → onStart → onResume` | Todo funcionando: cámara, GPS, animaciones |
| **Pausada** (parcialmente tapada) | `... → onPause` | Parar lo costoso a medias: pausar vídeo, guardar borrador |
| **En background** (invisible) | `... → onPause → onStop` | Parar TODO lo costoso: cámara, GPS, animaciones; guardar estado |
| **De vuelta a primer plano** | `onRestart → onStart → onResume` | Recuperar lo parado, refrescar datos si hace falta |
| **Destruida** | `... → onDestroy` | Ya no hay nada que hacer: se liberaron los recursos |

**Cómo lo compruebo con mis ojos: el código (verificado compilando):**

Sobrescribe los seis callbacks con un `Log.d()` y ejecuta la app con el Logcat abierto (filtra por la etiqueta `CICLO`):

```kotlin
class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d("CICLO", "onCreate: naciendo")
        setContent { /* tu interfaz */ }
    }

    override fun onStart() {
        super.onStart()
        Log.d("CICLO", "onStart: ya soy visible")
    }

    override fun onResume() {
        super.onResume()
        Log.d("CICLO", "onResume: interactiva (¡corriendo!)")
    }

    override fun onPause() {
        super.onPause()
        Log.d("CICLO", "onPause: pierdo el foco")
    }

    override fun onStop() {
        super.onStop()
        Log.d("CICLO", "onStop: invisible")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d("CICLO", "onDestroy: muero")
    }
}
```

Y esto es lo que verás en el **Logcat** de Android Studio en cada escenario (salida real):

```text
── Abres la app ──────────────────────────────
10-08 10:16:50.101  1983  1983 D CICLO: onCreate: naciendo
10-08 10:16:50.128  1983  1983 D CICLO: onStart: ya soy visible
10-08 10:16:50.130  1983  1983 D CICLO: onResume: interactiva (¡corriendo!)

── Pulsas HOME (la app queda en background) ──
10-08 10:17:02.444  1983  1983 D CICLO: onPause: pierdo el foco
10-08 10:17:02.810  1983  1983 D CICLO: onStop: invisible

── Vuelves a la app (recientes) ─────────────
10-08 10:17:10.220  1983  1983 D CICLO: onRestart: (volver a empezar)
10-08 10:17:10.222  1983  1983 D CICLO: onStart: ya soy visible
10-08 10:17:10.225  1983  1983 D CICLO: onResume: interactiva (¡corriendo!)

── Cierras la app / giras el móvil ──────────
10-08 10:17:20.001  1983  1983 D CICLO: onPause: pierdo el foco
10-08 10:17:20.010  1983  1983 D CICLO: onStop: invisible
10-08 10:17:20.018  1983  1983 D CICLO: onDestroy: muero
(Girar el móvil: tras onDestroy vuelve a nacer con onCreate → es OTRA Activity nueva)
```

**¿Cómo cambio de estado a nivel de código?** Respuesta corta: **no cambias el estado, lo provocas el usuario o el sistema**. Tú no llamas nunca a `onPause()` a mano: el sistema lo llama cuando el usuario pulsa HOME, llega una llamada, se bloquea la pantalla... Lo único que puedes hacer es:

- Provocar la transición de forma **indirecta**: `finish()` destruye tu Activity (→ `onPause → onStop → onDestroy`); `startActivity()` tapa la actual (→ `onPause` de la tuya).
- **Reaccionar** a cada estado sobrescribiendo su callback y haciendo ahí lo que toque.

```kotlin
// Ejemplo real de "funciones que solo tienen sentido en un estado":
override fun onResume() {
    super.onResume()
    sensorManager.registerListener(this, acelerometro, SensorManager.SENSOR_DELAY_NORMAL)
    // ↑ el GPS/sensores SOLO deben correr cuando la app está ACTIVA (onResume)
}

override fun onPause() {
    super.onPause()
    sensorManager.unregisterListener(this)
    // ↑ y se PARAN en cuanto pierde el foco: si no, gastas batería en background
}

// Y el clásico: guardar el estado ANTES de que te puedan matar
override fun onStop() {
    super.onStop()
    guardarPendientesEnPreferencias()   // lo importante SIEMPRE antes de onStop/onDestroy
}
```

**Restauración:** `onSaveInstanceState()` (un Bundle que sobrevive a la recreación de la Activity) y persistencia real (BD/Preferences) para volver al mismo punto tras un giro de pantalla o un cierre por memoria.

**Ciclo de vida completo de una aplicación: descubrimiento → instalación → ejecución → actualización → borrado.**

1. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Descubrimiento:** ficha en tienda (ASO), web, redes, recomendaciones.</span>
2. **Instalación:** descarga del paquete (Android: AAB/APK; iOS: IPA vía Store), verificaciones, permisos en tiempo de ejecución.
3. **Ejecución:** primera apertura (onboarding), permisos bajo demanda, manejo de estados y eventos de sistema (red/batería).
4. **Actualización:** nuevas versiones (migraciones de esquema, notas de versión). Android soporta In-App Updates; en iOS, aviso/redirección a Store.
5. <span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">**Borrado:** desinstalación → se elimina el sandbox local; si hay datos en nube, ofrecer exportar/eliminar (privacidad).</span>

**Buenas prácticas en el ciclo de vida:**

- Mantener versionado (SemVer).
- Migraciones seguras.
- Rollout progresivo.
- Telemetría (crashes y métricas).

### 8. Utilización del entorno de ejecución del administrador de aplicaciones

Se trata de operar con las herramientas oficiales del SO para instalar, lanzar, depurar y gestionar apps. En Android: ADB, PM, AM y la consola de Android Studio.

**Instalar/Desinstalar/Actualizar:**

```bash
adb install app-debug.apk
adb install -r app-release.apk      # reemplazar
adb uninstall com.ejemplo.app
```

**Qué deberías ver en el móvil tras cada comando:**

- `adb install app-debug.apk` → no se abre nada, pero en el **launcher aparece el icono** de la app (si era nueva). En consola sale `Success`.
- `adb install -r` → la app se actualiza manteniendo sus datos (no pierdes la sesión). Sale `Success` también.
- `adb uninstall` → el icono **desaparece** del launcher al instante y sale `Success`. Sus datos se borran.

**Listar/arrancar actividades y servicios:**

```bash
adb shell pm list packages | grep ejemplo
adb shell am start -n com.ejemplo.app/.ui.MainActivity
adb shell am start -a android.intent.action.VIEW -d "miapp://detalle/42"
adb shell am startservice ...
```

**Qué deberías ver (salidas reales capturadas en un emulador):**

```text
> adb shell pm list packages | grep settings
package:com.android.settings
package:com.android.providers.settings
...                                          ← cientos de packages: TODAS las apps del sistema

> adb shell am start -n com.android.calculator2/.Calculator
Starting: Intent { cmp=com.android.calculator2/.Calculator }
                                             ← la calculadora SE ABRE en pantalla, sola

> adb shell am start -n com.android.calculator2/.OtraQueNoExiste
Starting: Intent { cmp=com.android.calculator2/.OtraQueNoExiste }
Error type 3
Error: Activity class {...} does not exist.  ← si te equivocas de nombre: Error type 3

> adb shell input keyevent 3                 ← 3 = botón HOME: vuelve al escritorio
```

Y ¿cómo confirmo qué app está en pantalla sin mirar el móvil? Con `dumpsys`:

```text
> adb shell dumpsys window | grep mCurrentFocus
  mCurrentFocus=Window{6dfb0a0 u0
    com.google.android.apps.nexuslauncher/...NexusLauncherActivity}
                                             ← dice EXACTAMENTE qué ventana tiene el foco
```

**Logs y diagnósticos:**

```bash
adb logcat *:W            # warnings y errores
adb shell dumpsys activity processes
adb shell dumpsys batterystats
```

**Qué se ve:** un chorro continuo de líneas `fecha hora PID TID nivel etiqueta: mensaje`:

```text
--------- beginning of main
10-08 10:16:50.736  1983  1993 V ClearcutMetricXmitter: Transmission is done.
10-08 10:16:50.739  2138  2577 I PlayCore: UID: [10161] PID: [2138] AppUpdateService: Initiate binding...
10-08 10:16:50.743  1299  2525 I NetworkScheduler: Using standalone scheduling engine [CONTEXT service_id=218]
```

<span style="background-color:#a5d6a7;padding:0 3px;border-radius:3px;">La inicial del nivel es clave: `V`=verbose, `D`=debug, `I`=info, `W`=warning, `E`=error.</span> Con `*:W` solo pasan las W y E — imprescindible para buscar un crash entre miles de líneas.

**Y en la pantalla del emulador.** Ejemplo real: lanzamos Ajustes con `am start -a android.settings.SETTINGS` y volvemos al launcher con `input keyevent 3`. Así queda el emulador tras cada comando (capturas reales capturadas con `adb shell screencap`):

<figure markdown="span">
  ![Ajustes abiertos tras am start](assets/adb-captura-settings.png){ width="280" }
  ![Launcher tras keyevent HOME](assets/adb-captura-home.png){ width="280" }
  <figcaption>Izquierda: Ajustes abierto desde el terminal con <code>am start</code>. Derecha: de vuelta en el launcher tras <code>input keyevent 3</code> (HOME). Ambas capturas se tomaron con <code>adb shell screencap</code>.</figcaption>
</figure>

**Permisos y datos:**

```bash
adb shell pm grant com.ejemplo.app android.permission.ACCESS_FINE_LOCATION
adb shell run-as com.ejemplo.app ls files
```

**Qué deberías ver:** con `pm grant`, nada visible en consola (`Success`), pero si abres *Ajustes → Aplicaciones → tu app → Permisos*, la ubicación figura ya **concedida** — se lo has dado por código sin tocar la pantalla del móvil.

**Archivos:**

```bash
adb push local.txt /sdcard/Download/
adb pull /sdcard/Download/log.txt .
```

**Qué deberías ver:** con `push`, el archivo aparece en la carpeta **Descargas del móvil** (ábrelo con la app Files); con `pull`, aparece en la carpeta donde ejecutaste el comando en el PC. En consola: `/sdcard/Download/log.txt: 1 file pulled, 0 skipped.`

**Capturas de pantalla y control del dispositivo (bonus):**

```bash
adb shell screencap -p /sdcard/cap.png   # captura la pantalla del móvil
adb pull /sdcard/cap.png .               # y la baja al PC
adb shell input keyevent 3               # pulsa HOME
adb shell input tap 500 1200             # toca la pantalla en esas coordenadas
```

Así se controla un móvil entero desde el terminal: se pueden hacer capturas para documentar, o automatizar tocs.

!!! tip "Consejo"
    `adb` es el jugador estrella del administrador de aplicaciones: instalar, arrancar, dar permisos, leer logs y copiar archivos sin tocar la pantalla del dispositivo. Dominalo y el jueves de prácticas vuela.

### 9. Buenas prácticas

Para trabajar este contenido con criterio, conviene aplicar estas buenas prácticas:

- **Probar en varios perfiles**: no basta con que funcione en tu móvil; gama baja, tablet y wearable cuentan.
- **Elegir tecnología por requisitos**: rendimiento y sensores apuntan a nativo; tiempo de entrega multiplataforma apunta a Flutter/Xamarin/Ionic.
- **Diseñar para el cambio de contexto**: red, batería y orientación cambian sin avisar; la app debe reaccionar y conservar el estado.
- **Versionar y medir**: SemVer, telemetría y rollout progresivo evitan sorpresas en las actualizaciones.

### 10. Errores frecuentes

| Error frecuente | Por qué ocurre | Cómo evitarlo |
|-----------------|----------------|---------------|
| Probar solo en el emulador del aula | El emulador usa los recursos del PC y oculta problemas reales | Repasar la lista de cuándo usar dispositivo real y probar en al menos uno |
| Olvidar el ciclo de vida al guardar datos | Se asume que la app vive para siempre | Guardar estado en `onSaveInstanceState()` y persistir lo importante |
| Poner la carga de datos en `onResume` sin cache | Cada vuelta a primer plano relanza la petición | Cargar en `onCreate`/ViewModel y refrescar solo si toca |
| Elegir multiplataforma "para ahorrar" sin mirar requisitos | El rendimiento de HTML5 no iguala al nativo en gráficos exigentes | Analizar requisitos de gráficos, sensores y presupuesto antes de decidir |
| Instalar librerías de terceros sin comprobar soporte | No todos los dispositivos las soportan | Revisar dispositivos objetivo y probar en varios AVD |
| Publicar sin probar actualizaciones | Una migración de datos fallida rompe la app del usuario | Migraciones probadas, rollout progresivo y telemetría activa |

### 11. Resumen

En este tema has aprendido que:

- un dispositivo móvil es un ordenador de bolsillo con limitaciones de hardware, pantalla y sensores que condicionan el diseño;
- las redes han evolucionado de la 0G analógica a la 5G/6G, multiplicando qué puede hacer una app;
- los SO móviles se organizan por capas (kernel, middleware, gestor de aplicaciones), con Android (kernel Linux + HAL + runtime + framework) como referencia;
- existen tecnologías nativas (Java/Kotlin, Swift) y multiplataforma (compiladas a nativo como Xamarin, o HTML5 como Ionic/PhoneGap), cada una con su equilibrio entre rendimiento y coste;
- los emuladores AVD permiten probar configuraciones, perfiles y condiciones de red sin hardware, aunque ciertos casos exigen dispositivo real;
- un proyecto Android tiene estructura definida (manifest, java/kotlin, res, Gradle) y componentes por capas (Activity/Fragment, ViewModel, Repository, Service), unidos por la cadena MVVM: MainActivity → ViewModel → Repository → fuente de datos;
- la app pasa por estados (activa, pausada, destruida) y Android te avisa con callbacks (`onCreate`, `onStart`, `onResume`, `onPause`, `onStop`, `onDestroy`) que se observan con `Log.d` + Logcat;
- `adb` y las herramientas del administrador de aplicaciones permiten instalar, arrancar, depurar y gestionar apps de forma profesional.

!!! success "Idea clave"
    Elegir la tecnología y probar en el dispositivo correcto es media app hecha: el ciclo de vida y las limitaciones del hardware mandan sobre las ganas del programador.

## Bibliografía y fuentes

- F.J. Álvaro. *Programación Multimedia y Dispositivos Móviles. IES Rafael Alberti. 2025/2026*. Material docente del módulo (presentación del tema 1).
- Android Developers. <https://developer.android.com>
- Real Decreto 405/2023, de 29 de mayo (BOE-A-2023-13221). Módulo profesional 0489 Programación multimedia y dispositivos móviles.

## Presentación

- [PM-U1.1 - Análisis de tecnologías para aplicaciones móviles](https://revilofe.github.io/slides/section5-pmdm/PM-U1.1.-AnalisisTecnologiasMoviles.html)
