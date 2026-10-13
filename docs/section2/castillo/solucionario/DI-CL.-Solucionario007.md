---
title: "Solucionario Castillo 7"
description: El formulario optimizado con selector de fecha, desplegable y Snackbar; y el gestor de tareas con Scaffold completo.
summary: "Solución completa de la práctica Castillo 7."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s7
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 7

## Ejercicio 1 — Formulario optimizado

Cambios con su pauta (esto es lo que se evalúa, el antes/después):

| Defecto | Cambio aplicado | Pauta |
| ------- | --------------- | ----- |
| Navegación incoherente | Título "Registro" en `topBar`, Guardar pegado al contenido, Cancelar como `TextButton` | Estándares de plataforma |
| Fecha en texto libre | `DatePicker` de Material 3 | Control adecuado al dato |
| Profesión en texto libre | `ExposedDropdownMenuBox` | Control adecuado al dato |
| DNI con teclado alfabético | `KeyboardType.Number` | Prevención de errores |
| Tipografía plana | `titleLarge` / `bodyLarge` / `labelSmall` del tema | Legibilidad y jerarquía |
| Error en `AlertDialog` bloqueante | `Snackbar` con acción "Reintentar" | Mensajes en el momento |

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun FormularioOptimizado() {
    var nombre by remember { mutableStateOf("") }
    var fecha by remember { mutableStateOf<Long?>(null) }        // fecha REAL, no texto
    var profesion by remember { mutableStateOf("Estudiante") }
    var abierto by remember { mutableStateOf(false) }            // selector de fecha
    val snackbar = remember { SnackbarHostState() }
    val scope = rememberCoroutineScope()

    Scaffold(
        topBar = { TopAppBar(title = { Text("Registro") }) },    // título estándar
        snackbarHost = { SnackbarHost(snackbar) }                // avisos no bloqueantes
    ) { inner ->
        Column(Modifier.padding(inner).padding(16.dp),
               verticalArrangement = Arrangement.spacedBy(10.dp)) {

            OutlinedTextField(nombre, { nombre = it },
                label = { Text("Nombre y apellidos") })

            // fecha: botón que abre el DatePicker (control adecuado)
            OutlinedButton(onClick = { abierto = true }) {
                Text(fecha?.let {
                    DateFormat.getDateInstance().format(Date(it))
                } ?: "Fecha de nacimiento")
            }
            if (abierto) {
                DatePickerDialog(
                    onDismissRequest = { abierto = false },
                    confirmButton = { TextButton(onClick = { abierto = false }) { Text("OK") } }
                ) { DatePickerState(initialSelectedDateMillis = fecha)
                    .let { st -> fecha = st.selectedDateMillis } }
            }

            // profesión: desplegable readOnly (control adecuado)
            ExposedDropdownMenuBox(expanded = false, onExpandedChange = { }) {
                OutlinedTextField(profesion, {}, readOnly = true,
                    label = { Text("Profesión") },
                    modifier = Modifier.menuAnchor())
            }

            Button(onClick = {
                scope.launch {                                 // aviso no bloqueante
                    snackbar.showSnackbar("Registro guardado correctamente")
                }
            }, modifier = Modifier.fillMaxWidth()) { Text("Guardar") }
        }
    }
}
```

## Ejercicio 2 — Gestión de tareas (Scaffold completo)

```kotlin
data class Tarea(val texto: String, val hecha: Boolean = false)

@Composable
fun GestorTareas() {
    var nueva by remember { mutableStateOf("") }
    var soloPendientes by remember { mutableStateOf(false) }
    var borrar by remember { mutableStateOf<Tarea?>(null) }      // tarea a confirmar
    val tareas = remember { mutableStateListOf<Tarea>() }

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Mis tareas") },
                actions = {                                     // menú de la app bar
                    IconButton(onClick = { soloPendientes = !soloPendientes }) {
                        Icon(Icons.Filled.FilterList,
                             contentDescription = "Filtrar pendientes")
                    }
                    IconButton(onClick = { /* navegar a Ayuda */ }) {
                        Icon(Icons.AutoMirrored.Filled.HelpOutline, "Ayuda")
                    }
                }
            )
        },
        floatingActionButton = {                                // acción más usada
            FloatingActionButton(onClick = {
                if (nueva.isNotBlank()) { tareas.add(Tarea(nueva)); nueva = "" }
            }) { Icon(Icons.Filled.Add, "Nueva tarea") }
        }
    ) { inner ->
        Column(Modifier.padding(inner).padding(12.dp)) {
            OutlinedTextField(nueva, { nueva = it },
                label = { Text("Nueva tarea") }, modifier = Modifier.fillMaxWidth())

            val visibles = if (soloPendientes) tareas.filter { !it.hecha } else tareas
            LazyColumn {
                items(visibles) { t ->
                    Row(Modifier.fillMaxWidth()
                            .combinedClickable(
                                onClick = { },                   // un toque: nada
                                onLongClick = { borrar = t }     // mantener: menú contextual
                            )
                            .padding(vertical = 8.dp),
                        verticalAlignment = Alignment.CenterVertically) {
                        Checkbox(checked = t.hecha, onCheckedChange = { v ->
                            tareas[tareas.indexOf(t)] = t.copy(hecha = v)   // marcar
                        })
                        Text(t.text, modifier = Modifier.weight(1f),
                             style = if (t.hecha) MaterialTheme.typography.bodyLarge
                                     else MaterialTheme.typography.bodyLarge)
                    }
                }
            }
        }

        borrar?.let { t ->                                      // mensaje claro
            AlertDialog(
                onDismissRequest = { borrar = null },
                title = { Text("Eliminar tarea") },
                text = { Text("¿Seguro que deseas eliminar esta tarea? " +
                              "Esta acción no se puede deshacer.") },
                confirmButton = {
                    TextButton(onClick = { tareas.remove(t); borrar = null }) { Text("Eliminar") }
                },
                dismissButton = {
                    TextButton(onClick = { borrar = null }) { Text("Cancelar") }
                }
            )
        }
    }
}
```

**Patrones evaluables uno a uno:** app bar con título + acciones · menú contextual con
`combinedClickable(onLongClick)` · FAB para la acción frecuente · filtro con estado ·
`AlertDialog` con confirmación literalmente redactada. La versión Figma debe reflejar los
mismos cinco puntos en pantallas estáticas.
