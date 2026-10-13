---
title: "Solucionario Castillo 1: El entorno Android Studio"
description: Solución de la práctica Castillo 1 — manual de instalación, primer proyecto y los dos casos prácticos de la UD2 en Compose.
summary: "Checklist de instalación de Android Studio + código completo del login con navegación y del reproductor en rejilla 3x3."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s1
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 1

## Ejercicio 1 — Checklist del manual de instalación

El manual del alumno debe cubrir estos hitos (cada uno con captura):

1. Descarga desde `developer.android.com/studio` e instalación con el asistente
   (tipo "Standard": se trae SDK, plataformas y emulador).
2. **SDK Manager**: explicación de qué es un nivel de API y cuál instala (recomendado: el
   más reciente estable).
3. **Device Manager → Create Device**: elegir un teléfono (Pixel), una imagen de sistema
   (release sin Google Play vale para clase) y crear el AVD.
4. Primera ejecución del emulador.

## Ejercicio 2 — Primer proyecto

Nuevo proyecto → **Empty Activity (Compose)**. El alumno debe explicar con sus palabras:

- `MainActivity`: la actividad inicial, la "ventana" de la app.
- `setContent { ... }`: monta la interfaz de composables dentro de la actividad.
- `Greeting(...)`: el composable de ejemplo que genera la plantilla.

## Ejercicio 3 — Caso práctico 1: login

```kotlin
// Dos pantallas conectadas: login y bienvenida
@Composable
fun PantallaLogin(alAcceder: () -> Unit) {
    var usuario by remember { mutableStateOf("") }     // estado del campo usuario
    var clave by remember { mutableStateOf("") }       // estado del campo contraseña
    var error by remember { mutableStateOf(false) }    // ¿mostramos diálogo de error?

    Column(
        modifier = Modifier.fillMaxSize().padding(24.dp),   // relleno a los bordes
        verticalArrangement = Arrangement.spacedBy(12.dp)   // separación uniforme
    ) {
        Text("Acceso", style = MaterialTheme.typography.titleLarge)

        OutlinedTextField(                      // campo usuario
            value = usuario,                    // el texto viene del estado
            onValueChange = { usuario = it },   // cada tecla guarda en el estado
            label = { Text("Usuario") }         // etiqueta flotante
        )
        OutlinedTextField(
            value = clave,
            onValueChange = { clave = it },
            label = { Text("Contraseña") },
            visualTransformation = PasswordVisualTransformation(),  // puntos
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Password)
        )

        Button(
            onClick = {
                if (usuario == "Admin" && clave == "123456") alAcceder()  // credenciales
                else error = true                                            // credenciales malas
            },
            modifier = Modifier.fillMaxWidth()
        ) { Text("Entrar") }

        if (error) {   // mientras el estado lo pida, se pinta el diálogo
            AlertDialog(
                onDismissRequest = { error = false },      // pulsar fuera cierra
                title = { Text("Error") },
                text = { Text("La información de login es incorrecta.") },
                confirmButton = {
                    TextButton(onClick = { error = false }) { Text("Aceptar") }
                }
            )
        }
    }
}

@Composable
fun PantallaBienvenida(alVolver: () -> Unit) {
    Column(
        modifier = Modifier.fillMaxSize(),
        verticalArrangement = Arrangement.Center,      // centrado vertical
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        Text("Te has logado correctamente", style = MaterialTheme.typography.titleLarge)
        Spacer(Modifier.height(16.dp))
        OutlinedButton(onClick = alVolver) { Text("Volver") }
    }
}
```

Y el `NavHost` que las conecta (teoría §6):

```kotlin
val nav = rememberNavController()
NavHost(nav, startDestination = "login") {
    composable("login") { PantallaLogin(alAcceder = { nav.navigate("bienvenida") }) }
    composable("bienvenida") { PantallaBienvenida(alVolver = { nav.popBackStack() }) }
}
```

## Ejercicio 4 — Caso práctico 2: reproductor en rejilla

```kotlin
@Composable
fun Reproductor() {
    val pistas = listOf("Pista 1", "Pista 2", "Pista 3",
                        "Pista 4", "Pista 5", "Pista 6",
                        "Pista 7", "Pista 8", "Pista 9")
    Column(Modifier.fillMaxSize().padding(16.dp)) {
        Text("Mi música", style = MaterialTheme.typography.titleLarge)
        Spacer(Modifier.height(12.dp))

        LazyVerticalGrid(                    // rejilla perezosa de 3 columnas
            columns = GridCells.Fixed(3),
            horizontalArrangement = Arrangement.spacedBy(8.dp),
            verticalArrangement = Arrangement.spacedBy(8.dp),
            modifier = Modifier.weight(1f)   // se come el alto sobrante
        ) {
            items(pistas) { pista ->
                Button(onClick = { /* reproducir(pista) */ },
                       modifier = Modifier.fillMaxWidth()) {
                    Text(pista)
                }
            }
        }

        Row(                                 // controles abajo, en horizontal
            modifier = Modifier.fillMaxWidth().padding(top = 12.dp),
            horizontalArrangement = Arrangement.Center
        ) {
            IconButton(onClick = { /* prev */ }) { Icon(Icons.Filled.SkipPrevious, "Anterior") }
            IconButton(onClick = { /* play */ })  { Icon(Icons.Filled.PlayArrow, "Reproducir") }
            IconButton(onClick = { /* next */ })  { Icon(Icons.Filled.SkipNext, "Siguiente") }
        }
    }
}
```

**Reto de ampliación** (el de la teoría): envolver todo en un `Scaffold` con `topBar` y que el
botón de stop pida confirmación con un `AlertDialog`.
