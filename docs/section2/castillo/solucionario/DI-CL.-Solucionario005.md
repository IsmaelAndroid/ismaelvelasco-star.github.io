---
title: "Solucionario Castillo 5"
description: Tests JUnit de la calculadora y de GestorDeContactos con validaciones, más la agenda Compose con exportación XML.
summary: "Solución completa de la práctica Castillo 5."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s5
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 5

## Ejercicio 1 — CalculadoraTest.kt

```kotlin
class CalculadoraTest {

    private lateinit var calc: Calculadora

    @Before fun preparar() { calc = Calculadora() }   // calculadora fresca por test

    @Test fun `dos mas tres da cinco`() {
        calc.numero(2); calc.operar('+')
        calc.numero(3)
        assertEquals(5.0, calc.operar('=')!!, 1e-9)
    }

    @Test fun `operaciones encadenadas`() {
        calc.numero(2); calc.operar('+')
        calc.numero(3); calc.operar('+')
        calc.numero(4)
        assertEquals(9.0, calc.operar('=')!!, 1e-9)      // 2 + 3 + 4
    }

    @Test fun `division por cero devuelve null`() {
        calc.numero(7); calc.operar('/')
        calc.numero(0)
        assertNull(calc.operar('='))                     // contrato: null, no crash
    }

    @Test fun `ce en mitad de operacion reinicia`() {
        calc.numero(9); calc.operar('×'); calc.numero(9)
        calc.ce()
        calc.numero(1); calc.operar('+'); calc.numero(1)
        assertEquals(2.0, calc.operar('=')!!, 1e-9)
    }

    @Test fun `numeros de varias cifras componen`() {
        listOf(1,2,3).forEach { calc.numero(it) }        // 1→12→123
        assertEquals(123.0, calc.acumuladoActual(), 1e-9)
    }
}
```

## Ejercicio 2 — Agenda MVC testeada

### El modelo (sin nada de Android: por eso se puede testear)

```kotlin
data class Contacto(
    var nombre: String,
    var apellido: String,
    var telefono: String,
    var correo: String
)

object Validador {
    private val RE_CORREO = Regex("^[\\w.+-]+@[\\w-]+\\.[\\w.]+$")
    private val RE_TELEFONO = Regex("^[679]\\d{8}$")     // 9 dígitos, empieza 6/7/9

    fun correoValido(c: String) = RE_CORREO.matches(c)
    fun telefonoValido(t: String) = RE_TELEFONO.matches(t)
}

class GestorDeContactos {
    private val lista = mutableListOf<Contacto>()

    fun agregar(c: Contacto): Boolean =
        Validador.correoValido(c.correo) &&
        Validador.telefonoValido(c.telefono) &&
        lista.add(c)                                     // solo entra si valida

    fun editar(original: Contacto, nuevo: Contacto): Boolean {
        val i = lista.indexOf(original)
        if (i < 0) return false
        lista[i] = nuevo; return true
    }

    fun eliminar(c: Contacto) = lista.remove(c)
    fun listar(): List<Contacto> = lista.toList()        // copia defensiva
    fun exportarXml(): String = lista.joinToString("\n") { c ->
        "  <contacto nombre=\"${c.nombre}\" apellido=\"${c.apellido}\" " +
        "telefono=\"${c.telefono}\" correo=\"${c.correo}\"/>"
    }.let { "<agenda>\n$it\n</agenda>" }
}
```

### GestorDeContactosTest.kt

```kotlin
class GestorDeContactosTest {
    private val g = GestorDeContactos()
    private fun prueba() = Contacto("Ana", "García", "656123456", "ana@correo.es")

    @Test fun `agrega un contacto valido`() {
        assertTrue(g.agregar(prueba()))
        assertEquals(1, g.listar().size)
    }

    @Test fun `rechaza correo mal formado`() {
        val c = prueba().copy(correo = "ana-correo")
        assertFalse(g.agregar(c))                        // no entra en la agenda
    }

    @Test fun `rechaza telefono mal formado`() {
        assertFalse(g.agregar(prueba().copy(telefono = "123")))
    }

    @Test fun `acepta telefonos que empiezan por 6, 7 y 9`() {
        assertTrue(g.agregar(prueba().copy(telefono = "611111111")))
        assertTrue(g.agregar(prueba().copy(telefono = "722222222")))
        assertTrue(g.agregar(prueba().copy(telefono = "933333333")))
    }

    @Test fun `edita y elimina`() {
        val c = prueba(); g.agregar(c)
        assertTrue(g.editar(c, c.copy(correo = "nueva@correo.es")))
        assertTrue(g.eliminar(c))
        assertTrue(g.listar().isEmpty())
    }

    @Test fun `exportar xml envuelve la lista`() {
        g.agregar(prueba())
        val xml = g.exportarXml()
        assertTrue(xml.startsWith("<agenda>") && xml.contains("<contacto "))
    }
}
```

### La vista (Compose) — pinta y delega, nada de lógica

```kotlin
@Composable
fun AgendaUI() {
    val gestor = remember { GestorDeContactos() }
    val contactos = remember { mutableStateListOf<Contacto>() }   // espejo observable
    var nombre by remember { mutableStateOf("") }; var apellido by remember { mutableStateOf("") }
    var tel by remember { mutableStateOf("") };    var correo by remember { mutableStateOf("") }

    Column(Modifier.padding(16.dp), verticalArrangement = Arrangement.spacedBy(8.dp)) {
        OutlinedTextField(nombre, { nombre = it }, label = { Text("Nombre") })
        OutlinedTextField(apellido, { apellido = it }, label = { Text("Apellido") })
        OutlinedTextField(tel, { tel = it }, label = { Text("Teléfono") })
        OutlinedTextField(correo, { correo = it }, label = { Text("Correo") })

        Button(onClick = {
            if (gestor.agregar(Contacto(nombre, apellido, tel, correo))) {
                contactos.clear(); contactos.addAll(gestor.listar())  // refresca
                nombre = ""; apellido = ""; tel = ""; correo = ""
            }
        }) { Text("Agregar") }

        LazyColumn {
            items(contactos) { c ->
                Card(Modifier.fillMaxWidth().padding(vertical = 4.dp)) {
                    Row(Modifier.padding(12.dp),
                        horizontalArrangement = Arrangement.SpaceBetween) {
                        Column { Text("${c.nombre} ${c.apellido}")
                                 Text(c.correo, style = MaterialTheme.typography.bodySmall) }
                        IconButton(onClick = { gestor.eliminar(c)
                            contactos.clear(); contactos.addAll(gestor.listar()) }) {
                            Icon(Icons.Filled.Delete, "Eliminar")
                        }
                    }
                }
            }
        }
    }
}
```

**Criterio estrella:** quien mete la validación en el `onClick` en vez de en el modelo,
repite la práctica. El test de la vista no existe; el del modelo, sí.
