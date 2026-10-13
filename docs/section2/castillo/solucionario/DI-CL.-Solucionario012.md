---
title: "Solucionario Castillo 12"
description: Tabla caja negra/caja blanca, guion de depuración con Logcat y breakpoints, suite de regresión y el carrito testeado.
summary: "Solución completa de la práctica Castillo 12."
authors:
    - Ismael Velasco
date: 2026-10-13
icon: "material/check-circle"
permalink: /di/castillo/s12
categories:
    - DI
tags:
    - DI
    - Solucionario
---

# Solucionario Castillo 12

## Ejercicio 1 — Clasificación (contenido mínimo del Genially)

| | **Caja negra** | **Caja blanca** |
| -- | -------------- | --------------- |
| Qué mira | entradas → salidas, sin ver el código | la estructura interna del código |
| Quién la hace | tester / usuario | el desarrollador |
| Busca | funciones que fallan (requisitos) | caminos, condiciones, bucles sin cubrir |
| Ejemplos | probar que `7 ÷ 0` muestra "Error"; probar el flujo completo de login | `Calculadora.operar` con los 4 operadores + encadenados; cobertura del `when` |
| Técnica típica | particiones de equivalencia, valores límite | cobertura de código, complexidad ciclomática |

Importancia: la negra valida lo que el USUARIO espera; la blanca lo que el CÓDIGO hace.
Las dos juntas son la única verdad.

## Ejercicio 2 — Depuración documentada (guion)

**Error de ejecución** (crash):

```kotlin
fun operar(op: Char): Double? = when (op) {
    '/' -> acumulado / pendiente        // 💥 ArithmeticException si pendiente = 0
    ...
}
```

1. Al pulsar `÷ 0 =` la app se cierra. **Logcat** (filtro `package:es.iesra`) muestra:

```
FATAL EXCEPTION: main
java.lang.ArithmeticException: divide by zero
    at es.iesra.calculadora.Calculadora.operar(Calculadora.kt:34)
```

2. Clic en `Calculadora.kt:34` → línea exacta.
3. Fix: devolver `null` si `pendiente == 0.0` y que la UI muestre "Error".
4. Captura del before (crash) y del after (Error en pantalla).

**Error de lógica** (no peta, miente):

```kotlin
fun aplicarIva(base: Double) = base * 0.21     // devuelve SOLO el IVA, no el total
```

1. No hay excepción: hay que **sospechar del resultado** (síntoma: totales bajísimos).
2. **Breakpoint** en la línea → botón Debug → *Evaluate Expression*:
   `aplicarIva(100.0)` devuelve `21.0` cuando el usuario esperaba `121.0`.
3. Fix: `base * 1.21` (o documentar que devuelve solo la cuota).
4. Captura del breakpoint con el valor inspeccionado.

## Ejercicio 3 — Regresión

1. Funcionalidad nueva en `Calculadora`: `fun porcentaje(base: Double) = base / 100`.
2. Tests NUEVOS (`porcentaje()`) + tests VIEJOS intactos (los de Castillo 5).
3. Resultado esperado en la ventana de Gradle:

```
BUILD SUCCESSFUL in 6s
12 tests completed, 12 passed      ← 9 viejos + 3 nuevos: CERO regresión
```

Si un test viejo rompe con el cambio nuevo: eso ES una regresión, y se documenta igual
(qué rompió, por qué, cómo se reconcilió).

## Ejercicio 4 — El carrito testeado

```kotlin
data class Producto(val nombre: String, val precio: Double, val cantidad: Int = 1)

class Carrito {
    private val lineas = mutableListOf<Producto>()

    fun agregar(p: Producto) {                     // añade (o suma cantidad si existe)
        lineas.find { it.nombre == p.nombre }?.let {
            lineas[lineas.indexOf(it)] = it.copy(cantidad = it.cantidad + p.cantidad)
        } ?: lineas.add(p)
    }

    fun eliminar(nombre: String) {
        lineas.removeAll { it.nombre == nombre }
    }

    fun total(): Double = lineas.sumOf { it.precio * it.cantidad }  // POCO
}
```

```kotlin
class CarritoTest {
    private val c = Carrito()

    @Test fun `agregar un producto`() {
        c.agregar(Producto("Teclado", 29.99))
        assertEquals(1, c.lineasVisibles().size)
    }

    @Test fun `agregar dos veces el mismo suma cantidades`() {
        c.agregar(Producto("Ratón", 9.99)); c.agregar(Producto("Ratón", 9.99))
        assertEquals(19.98, c.total(), 1e-9)
    }

    @Test fun `eliminar un producto`() {
        c.agregar(Producto("Monitor", 199.0))
        c.eliminar("Monitor")
        assertEquals(0.0, c.total(), 1e-9)
    }

    @Test fun `el total suma linea a linea`() {
        c.agregar(Producto("Cable", 3.5, cantidad = 4))       // 14
        c.agregar(Producto("Hub", 24.0))                      // 24
        assertEquals(38.0, c.total(), 1e-9)
    }
}
```

**Ampliación (cupón):** `fun totalConCupon(pct: Int) = total() * (100 - pct) / 100` y tests
con 0% (= total), 100% (= 0) y el caso frontera negativo (¿lanza o clampa?).
