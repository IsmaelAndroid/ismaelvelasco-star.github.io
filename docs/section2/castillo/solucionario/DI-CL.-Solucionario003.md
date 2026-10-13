---
title: "Solucionario Castillo 3"
description: Traducción a Compose de los tres layouts XML: formulario, calculadora y lista de tareas.
summary: "Solución completa de la práctica Castillo 3."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s3
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 3

## Ejercicio 1 — Formulario

La traducción uno a uno (misma jerarquía, herramientas nuevas):

```kotlin
@Composable
fun RegistroDesdeXml() {
    var nombre by remember { mutableStateOf("") }
    var correo by remember { mutableStateOf("") }
    var clave  by remember { mutableStateOf("") }
    var perfil by remember { mutableStateOf("Alumno") }      // RadioGroup → una variable

    Column(Modifier.padding(16.dp), verticalArrangement = Arrangement.spacedBy(10.dp)) {
        // EditText hint="Nombre"  →  OutlinedTextField + label
        OutlinedTextField(nombre, { nombre = it }, label = { Text("Nombre completo") })

        // inputType="textEmailAddress"  →  KeyboardOptions
        OutlinedTextField(correo, { correo = it }, label = { Text("Correo") },
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Email))

        // inputType="textPassword"  →  PasswordVisualTransformation
        OutlinedTextField(clave, { clave = it }, label = { Text("Contraseña") },
            visualTransformation = PasswordVisualTransformation())

        Text("Perfil")
        Row {
            listOf("Alumno", "Profesor").forEach { op ->        // RadioGroup
                Row(verticalAlignment = Alignment.CenterVertically) {
                    RadioButton(selected = perfil == op, onClick = { perfil = op })
                    Text(op)
                }
            }
        }

        Button(onClick = { /* registrar */ }, modifier = Modifier.fillMaxWidth()) {
            Text("Registrar")
        }
    }
}
```

## Ejercicio 2 — Calculadora (reutilizando la lógica)

La interfaz cambia, la clase `Calculadora` **ni se toca**:

```kotlin
@Composable
fun CalculadoraDesdeXml() {
    val calc = remember { Calculadora() }              // ← la MISMA clase de Castillo 2
    var display by remember { mutableStateOf("0") }

    Column(Modifier.padding(12.dp), verticalArrangement = Arrangement.spacedBy(8.dp)) {
        // TextView gravity="right"  →  textAlign
        Text(display, style = MaterialTheme.typography.headlineMedium,
             modifier = Modifier.fillMaxWidth(), textAlign = TextAlign.End)

        val teclas = listOf(
            listOf("7","8","9","÷"), listOf("4","5","6","×"),
            listOf("1","2","3","−"), listOf("CE","0","=","+"))

        teclas.forEach { fila ->
            Row(horizontalArrangement = Arrangement.spacedBy(8.dp)) {
                fila.forEach { t ->
                    Button(onClick = {
                        display = when {
                            t[0].isDigit() -> { calc.numero(t.toInt()); t }
                            t == "CE"      -> { calc.ce(); "0" }
                            else -> (calc.operar(
                                when (t) { "÷"->'/'; "×"->'*'; "−"->'-'; else -> t[0] }
                            ) ?: "Error").toString()
                        }
                    }, modifier = Modifier.weight(1f)) { Text(t) }
                }
            }
        }
    }
}
```

**Aquí está el corazoncito de la práctica:** el alumno que copia-pega la UI entera de la
práctica 2 en vez de reutilizar `Calculadora` pierde el punto de reutilización. La interfaz
es desechable; la lógica es patrimonio.

## Ejercicio 3 — Lista de tareas (ListView → LazyColumn)

```kotlin
@Composable
fun ListaTareas() {
    var texto by remember { mutableStateOf("") }
    val tareas = remember { mutableStateListOf<String>() }   // lista OBSERVABLE

    Column(Modifier.padding(12.dp), verticalArrangement = Arrangement.spacedBy(10.dp)) {
        Row(horizontalArrangement = Arrangement.spacedBy(8.dp),
            verticalAlignment = Alignment.CenterVertically) {
            OutlinedTextField(texto, { texto = it },
                label = { Text("Escribe una tarea...") }, modifier = Modifier.weight(1f))
            Button(onClick = {
                if (texto.isNotBlank()) { tareas.add(texto); texto = "" }  // añadir
            }) { Text("Añadir") }
        }

        LazyColumn(verticalArrangement = Arrangement.spacedBy(6.dp)) {
            items(tareas) { tarea ->
                Row(Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween,
                    verticalAlignment = Alignment.CenterVertically) {
                    Text(tarea, modifier = Modifier.weight(1f))
                    IconButton(onClick = { tareas.remove(tarea) }) {       // eliminar
                        Icon(Icons.Filled.Delete, contentDescription = "Eliminar $tarea")
                    }
                }
            }
        }
    }
}
```

**Claves:** `mutableStateListOf` hace que añadir/eliminar **redibuje** la lista solo (el
equivalente de llamar a `notifyDataSetChanged` en la era XML, pero sin ceremonia) ·
`LazyColumn` solo compone las filas visibles (por eso sustituyó a `ListView`).
