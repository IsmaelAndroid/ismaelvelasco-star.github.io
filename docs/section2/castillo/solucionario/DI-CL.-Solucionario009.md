---
title: "Solucionario Castillo 9"
description: Informe de contactos con filtro en vivo, orden configurable y exportación CSV desde el modelo.
summary: "Solución completa de la práctica Castillo 9."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s9
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 9

## El modelo gana dos métodos (la vista no sabe nada de CSV)

```kotlin
// dentro de GestorDeContactos
fun filtrar(prefijo: String, orden: String): List<Contacto> {
    var r = if (prefijo.isBlank()) listar()
            else listar().filter { it.apellido.startsWith(prefijo, ignoreCase = true) }
    r = when (orden) {
        "Nombre"  -> r.sortedBy { it.nombre }
        "Teléfono"-> r.sortedBy { it.telefono }
        else      -> r.sortedBy { it.apellido }        // orden por defecto
    }
    return r
}

fun exportarCsv(contactos: List<Contacto>): String =
    "nombre;apellido;telefono;correo\n" +               // cabecera del informe
    contactos.joinToString("\n") { "${it.nombre};${it.apellido};${it.telefono};${it.correo}" }
```

## La pantalla de informe

```kotlin
@Composable
fun InformeContactos(gestor: GestorDeContactos) {
    var filtro by remember { mutableStateOf("") }
    var orden by remember { mutableStateOf("Apellido") }
    val ordenes = listOf("Apellido", "Nombre", "Teléfono")

    val resultados = remember(filtro, orden) {           // recalcula al cambiar criterios
        gestor.filtrar(filtro, orden)
    }

    Scaffold(topBar = { TopAppBar(title = { Text("Informe de contactos") }) }) { inner ->
        Column(Modifier.padding(inner).padding(12.dp),
               verticalArrangement = Arrangement.spacedBy(8.dp)) {

            OutlinedTextField(filtro, { filtro = it },
                label = { Text("Filtrar por apellido") }, modifier = Modifier.fillMaxWidth())

            Row(verticalAlignment = Alignment.CenterVertically) {
                Text("Orden: "); Spacer(Modifier.width(8.dp))
                ordenes.forEach { o ->
                    Row(verticalAlignment = Alignment.CenterVertically) {
                        RadioButton(selected = orden == o, onClick = { orden = o }); Text(o)
                    }
                }
            }

            Text("Mostrando ${resultados.size} de ${gestor.listar().size} contactos",
                 style = MaterialTheme.typography.labelMedium)   // contador del informe

            LazyColumn(Modifier.weight(1f)) {
                items(resultados) { c ->
                    Card(Modifier.fillMaxWidth().padding(vertical = 4.dp)) {
                        Column(Modifier.padding(12.dp)) {
                            Text("${c.apellido}, ${c.nombre}",
                                 style = MaterialTheme.typography.titleSmall)
                            Text("${c.telefono} · ${c.correo}",
                                 style = MaterialTheme.typography.bodySmall)
                        }
                    }
                }
            }

            OutlinedButton(onClick = {
                val csv = gestor.exportarCsv(resultados)          // exporta lo FILTRADO
                context.openFileOutput("informe.csv", MODE_PRIVATE)
                    .use { it.write(csv.toByteArray()) }           // guardado en la app
            }, modifier = Modifier.fillMaxWidth()) { Text("Exportar CSV") }
        }
    }
}
```

> Nota: para escribir el fichero hace falta el `context` local
> (`val context = LocalContext.current`) y el `import
> android.content.Context.MODE_PRIVATE`. Para **compartirlo** en vez de guardarlo, la vía
> 2026 es `FileProvider` + `Intent.ACTION_SEND`; se acepta como ampliación.

**Criterios de corrección:** el filtro reacciona mientras se escribe (`remember(filtro,
orden)`) · el contador es coherente con el filtro · la exportación exporta **lo que se ve**
y vive en el **modelo**, no en la UI.
