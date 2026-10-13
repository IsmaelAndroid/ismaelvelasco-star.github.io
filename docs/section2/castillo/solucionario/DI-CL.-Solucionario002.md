---
title: "Solucionario Castillo 2"
description: Código Compose del login con diálogos, la calculadora con lógica separada y el formulario completo con resumen.
summary: "Solución completa de la práctica Castillo 2 en Jetpack Compose."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s2
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 2

## Ejercicio 1 — Login con diálogos

```kotlin
@Composable
fun Login() {
    var usuario by remember { mutableStateOf("") }
    var clave   by remember { mutableStateOf("") }
    var dlg     by remember { mutableStateOf<String?>(null) }   // null = sin diálogo

    Column(Modifier.padding(24.dp), verticalArrangement = Arrangement.spacedBy(12.dp)) {
        OutlinedTextField(usuario, { usuario = it }, label = { Text("Usuario") })
        OutlinedTextField(clave, { clave = it }, label = { Text("Contraseña") },
            visualTransformation = PasswordVisualTransformation())

        Row(horizontalArrangement = Arrangement.spacedBy(12.dp)) {
            Button(onClick = {
                dlg = if (usuario == "Admin" && clave == "123456") "ok" else "ko"
            }) { Text("Entrar") }

            OutlinedButton(onClick = { usuario = ""; clave = "" }) { Text("Cancelar") }
        }
    }

    dlg?.let { tipo ->                       // si el estado pide diálogo, se pinta
        AlertDialog(
            onDismissRequest = { dlg = null },
            title = { Text(if (tipo == "ok") "Bienvenido" else "Error") },
            text  = { Text(if (tipo == "ok")
                        "Se ha logado correctamente"
                        else "La información de login es incorrecta") },
            confirmButton = { TextButton(onClick = { dlg = null }) { Text("Aceptar") } }
        )
    }
}
```

## Ejercicio 2 — Calculadora (lógica + interfaz)

`Calculadora.kt` — la lógica vive AQUÍ, sin nada de interfaz:

```kotlin
class Calculadora {
    private var acumulado = 0.0        // lo que llevamos acumulado
    private var operacion: Char? = null// operación pendiente ('+','-','*','/')

    fun numero(n: Int) = acumulado = acumulado * 10 + n   // teclear dígito

    fun operar(op: Char): Double? {    // pulsa +,-,*,/ o =
        val resultado = when (operacion) {
            '+' -> acumulado + pendiente
            '-' -> acumulado - pendiente
            '*' -> acumulado * pendiente
            '/' -> if (pendiente != 0.0) acumulado / pendiente else Double.NaN
            else -> pendiente
        }
        acumulado = resultado
        operacion = if (op == '=') null else op
        return if (resultado.isNaN()) null else resultado
    }
    // ...versión completa con memoria de operando pendiente en la teoría §7
    fun ce() { acumulado = 0.0; operacion = null }
}
```

> Nota del profesor: la versión íntegra usa dos campos (`acumulado` y `pendiente`) para
> encadenar operaciones tipo `2 + 3 + 4`. La de arriba es el esqueleto mínimo evaluable.

`CalculadoraUI.kt` — la interfaz solo pinta y delega:

```kotlin
@Composable
fun CalculadoraUI() {
    val calc = remember { Calculadora() }        // la lógica, una sola vez
    var pantalla by remember { mutableStateOf("0") }

    Column(Modifier.padding(16.dp), verticalArrangement = Arrangement.spacedBy(8.dp)) {
        Text(pantalla, style = MaterialTheme.typography.headlineMedium,
             modifier = Modifier.fillMaxWidth(), textAlign = TextAlign.End)

        val filas = listOf(
            listOf("7","8","9","÷"), listOf("4","5","6","×"),
            listOf("1","2","3","−"), listOf("CE","0","=","+")
        )
        filas.forEach { fila ->
            Row(horizontalArrangement = Arrangement.spacedBy(8.dp)) {
                fila.forEach { tecla ->
                    Button(
                        onClick = {
                            pantalla = when {
                                tecla[0].isDigit() -> { calc.numero(tecla.toInt()); tecla }
                                tecla == "CE"      -> { calc.ce(); "0" }
                                else               -> (calc.operar(
                                    when (tecla) { "÷"->'/'; "×"->'*'; "−"->'-'; else->tecla[0] }
                                ) ?: "Error").toString()
                            }
                        },
                        modifier = Modifier.weight(1f)
                    ) { Text(tecla) }
                }
            }
        }
    }
}
```

## Ejercicio 3 — Formulario de datos personales

```kotlin
@Composable
fun Formulario() {
    var nombre by remember { mutableStateOf("") }
    var fecha  by remember { mutableStateOf("") }
    var dni    by remember { mutableStateOf("") }
    var sexo   by remember { mutableStateOf("H") }          // radio compartido
    var correo by remember { mutableStateOf("") }
    var profesion by remember { mutableStateOf("Estudiante") }
    var comercios by remember { mutableStateOf(false) }     // checkbox
    var resumen by remember { mutableStateOf<String?>(null) }

    val profesiones = listOf("Estudiante", "Docente", "Técnico", "Otros")
    val expandible  by remember { mutableStateOf(false) }   // estado del desplegable

    Column(Modifier.padding(16.dp).verticalScroll(rememberScrollState()),
           verticalArrangement = Arrangement.spacedBy(10.dp)) {

        OutlinedTextField(nombre, { nombre = it }, label = { Text("Nombre y apellidos") })
        OutlinedTextField(fecha,  { fecha  = it }, label = { Text("Fecha de nacimiento (dd/mm/aaaa)") })
        OutlinedTextField(dni,    { dni    = it }, label = { Text("DNI") })
        OutlinedTextField(correo, { correo = it }, label = { Text("Correo electrónico") })

        Text("Sexo")
        Row {
            listOf("H" to "Hombre", "M" to "Mujer", "X" to "No binario").forEach { (v, et) ->
                Row(verticalAlignment = Alignment.CenterVertically) {
                    RadioButton(selected = sexo == v, onClick = { sexo = v })  // comparten variable
                    Text(et)
                }
            }
        }

        ExposedDropdownMenuBox(expanded = expandible,
                               onExpandedChange = { /* toggle */ }) {
            OutlinedTextField(profesion, {}, readOnly = true,
                label = { Text("Profesión") }, trailingIcon = { /* flecha */ })
            ExposedDropdownMenu(expanded = expandible, onDismissRequest = { }) {
                profesiones.forEach { p ->
                    DropdownMenuItem(text = { Text(p) }, onClick = { profesion = p })
                }
            }
        }

        Row(verticalAlignment = Alignment.CenterVertically) {
            Checkbox(checked = comercios, onCheckedChange = { comercios = it })
            Text("Acepto recibir comunicaciones comerciales")
        }

        Row(horizontalArrangement = Arrangement.spacedBy(12.dp)) {
            Button(onClick = {
                resumen = "Nombre: $nombre\nFecha: $fecha\nDNI: $dni\n" +
                          "Sexo: $sexo\nCorreo: $correo\nProfesión: $profesion\n" +
                          "Comerciales: ${if (comercios) "sí" else "no"}"
            }) { Text("Guardar") }
            OutlinedButton(onClick = { nombre=""; fecha=""; dni=""; correo=""; comercios=false }) {
                Text("Cancelar")
            }
        }

        resumen?.let {
            AlertDialog(onDismissRequest = { resumen = null },
                title = { Text("Datos guardados") },
                text  = { Text(it) },
                confirmButton = { TextButton(onClick = { resumen = null }) { Text("OK") } })
        }
    }
}
```

**Puntos de evaluación:** cada campo con su estado · radios compartiendo variable (§7.5) ·
`readOnly` en el combo (§7.6) · el resumen construido por interpolación de cadenas.
