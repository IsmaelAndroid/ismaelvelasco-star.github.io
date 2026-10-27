# PM-U3 - Navegación, estado y red

Note: Unidad 3 completa en un deck. Es el salto de "pantallas sueltas" a una app con memoria: navegar con datos, guardar estado, persistir en local y hablar con un servidor. Todo lo que se cuenta aquí está en los cinco temas de la unidad.

---

## Índice de la unidad

Note: Seguir el orden del temario. Los cinco temas se apoyan uno en otro: sin navegación no hay paso de argumentos, sin estado no hay Room que observar, y sin Room no hay nada que sincronizar con la red.

- **3.1** Navegación: grafo, argumentos y deep links
- **3.2** Estado y formularios: ViewModel, StateFlow y validación
- **3.3** Room: entidades, DAO y Flow
- **3.4** Retrofit: red, DTOs y errores
- **3.5** Taller integrador: sesión con DataStore y pruebas

---

## 3.1 · El grafo de navegación

Note: El grafo es un mapa de destinos, no una lista de pantallas. Cada destino tiene una ruta única. La ventaja frente a Activities: una sola Activity, el estado no se pierde al cambiar de pantalla.

- Un **destino** = una ruta con nombre (`"detalle/{id}"`)
- `NavHost` es el contenedor; `composable()` registra cada pantalla
- `NavController` navega (`navigate`) y desapila (`popBackStack`)
- Una sola Activity: no se destruye la pantalla al cambiar

---

## 3.1 · Argumentos: pasar datos sin romper

Note: El argumento se declara en la ruta y se recibe tipado. Si el argumento no existe, la app no debe romper: se declara nullable y se comprueba. Esto entra en la práctica como caso de prueba.

```kotlin
composable(
    route = "detalle/{id}",                    // ruta con hueco
    arguments = listOf(navArgument("id") {     // declara el argumento
        type = NavType.IntType                 // tipado, no string suelto
    })
) { entrada ->
    val id = entrada.arguments?.getInt("id") ?: 0   // valor por defecto
    PantallaDetalle(id)                            // pantalla con el dato
}
```

- Argumento **obligatorio** vs **nullable**: el nullable se comprueba antes de usarlo
- Nunca pasar objetos grandes por la ruta: se pasa el **id** y se consulta

---

## 3.1 · La pila y los deep links

Note: La pila es el historial. popBackStack vuelve atrás; popUpTo con inclusive limpia el historial al cambiar de sección. Los deep links abren una pantalla concreta desde fuera: notificación, web, WhatsApp.

| Acción | Efecto |
|---|---|
| `navigate("detalle/7")` | apila la pantalla de detalle |
| `popBackStack()` | vuelve a la anterior |
| `popUpTo(inicio) { inclusive = true }` | limpia el historial |
| `navDeepLink { uriPattern = "miapp://detalle/{id}" }` | abre desde fuera |

- El deep link se prueba con `adb shell am start -a android.intent.action.VIEW -d "miapp://detalle/7"`

---

## 3.2 · El problema del estado en la pantalla

Note: Si el estado vive en la pantalla, muere con ella: al girar el móvil se pierde el formulario. El ViewModel sobrevive a la rotación porque vive más que la composición. Es la razón de ser del patrón.

- Estado en la pantalla → **se pierde al rotar** o al cambiar de destino
- `ViewModel` → sobrevive a la rotación y a la recomposición
- La pantalla **observa** el estado, no lo posee
- El ViewModel no conoce Compose: no importa `@Composable`

---

## 3.2 · StateFlow: un solo dueño del estado

Note: El ViewModel expone un StateFlow inmutable; la pantalla lo convierte en State con collectAsState. Se escribe con una función, nunca desde fuera. Eso evita que dos pantallas cambien el mismo dato a la vez.

```kotlin
class FormularioViewModel : ViewModel() {
    private val _estado = MutableStateFlow(EstadoFormulario())  // privado
    val estado: StateFlow<EstadoFormulario> = _estado.asStateFlow()  // solo lectura

    fun alCambiarNombre(texto: String) {                        // única vía de cambio
        _estado.update { it.copy(nombre = texto) }              // copia inmutable
    }
}
```

- `_estado` privado + `asStateFlow()` público = **una sola vía de escritura**
- En la pantalla: `val estado by viewModel.estado.collectAsState()`

---

## 3.2 · Validar en el ViewModel (y avisar con Snackbar)

Note: La validación va donde vive el estado, no en el onClick del botón. Así se puede probar sin interfaz. El Snackbar necesita un Scaffold con snackbarHostState y una corrutina.

- La validación se hace **en el ViewModel**: se puede probar sin pantalla
- El error se guarda en el estado (`errorNombre: String?`) y la UI lo pinta
- El botón solo llama a `enviar()`; no valida
- El aviso se muestra con `SnackbarHostState.showSnackbar(...)` dentro de `rememberCoroutineScope()`

---

## 3.3 · Room: por qué no SQLite a pelo

Note: Room es una capa sobre SQLite que comprueba las consultas en tiempo de compilación. Si escribes mal un nombre de columna, el proyecto no compila: ese es el argumento real frente a SQLite directo.

| Sin Room | Con Room |
|---|---|
| SQL en cadenas, falla en ejecución | consultas **verificadas al compilar** |
| cursores a mano | devuelve objetos |
| sin aviso al cambiar la tabla | migraciones declaradas |

- Tres piezas: **entidad** (tabla), **DAO** (consultas), **base de datos** (punto de acceso)

---

## 3.3 · Entidad, DAO y base de datos

Note: La entidad es una clase con @Entity; cada propiedad es una columna y la clave primaria se declara. El DAO es una interfaz: los métodos son consultas. La base de datos es un singleton: solo puede haber una instancia.

```kotlin
@Entity(tableName = "tareas")                        // una clase = una tabla
data class Tarea(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,  // clave primaria
    val titulo: String,                                 // columna TEXT
    val hecha: Boolean = false                          // columna INTEGER
)

@Dao
interface TareaDao {
    @Query("SELECT * FROM tareas ORDER BY id DESC")     // consulta verificada
    fun observar(): Flow<List<Tarea>>                   // Flow: avisa al cambiar
}
```

- `Flow` en el DAO = la pantalla se **actualiza sola** cuando cambia la tabla

---

## 3.3 · Migraciones: cambiar sin perder datos

Note: Cambiar la tabla sin migración borra los datos o rompe la app. La migración se declara con número de versión y el SQL del cambio. En clase se prueba con un dato real antes y después.

- Subir `version` sin migración = **pérdida de datos** (o crash)
- `Migration(1, 2)` contiene el SQL del cambio (`ALTER TABLE ...`)
- Se prueba con un dato real: crear, migrar, comprobar que sigue ahí
- En desarrollo se puede usar `fallbackToDestructiveMigration()`, **nunca en producción**

---

## 3.4 · Retrofit: tres piezas y un responsable cada una

Note: DTO = lo que devuelve el servidor. Interfaz = la llamada declarada. Cliente = Retrofit con el conversor JSON. Si se mezclan, el código acaba con la red dentro de la pantalla.

| Pieza | Responsabilidad | Error típico |
|---|---|---|
| **DTO** | forma exacta del JSON | añadir campos que el servidor no manda |
| **Interfaz** | declarar la llamada (`@GET`) | poner la URL entera en cada método |
| **Cliente** | Retrofit + conversor | crear un Retrofit por llamada |

- La pantalla **no** hace red: pide al repositorio/ViewModel y observa estados

---

## 3.4 · La pantalla tiene tres estados, no uno

Note: Cargando, error y datos. Si solo se pinta el caso bueno, la app se queda en blanco cuando falla la red. Esto es criterio de evaluación: los tres estados tienen que verse.

```kotlin
sealed interface EstadoUi {                 // estados excluyentes
    data object Cargando : EstadoUi         // aún no hay datos
    data class Error(val mensaje: String) : EstadoUi  // fallo con mensaje
    data class Datos(val lista: List<Tarea>) : EstadoUi // caso bueno
}
```

- `when (estado)` en la pantalla: un `CircularProgressIndicator`, un mensaje con reintento y la lista
- Un `null` como estado = el fallo clásico: no distingue "cargando" de "vacío"

---

## 3.5 · El taller: sesión que sobrevive y pruebas

Note: DataStore guarda la sesión en disco: sobrevive al cierre de la app. Las pruebas demuestran que la validación funciona sin abrir la pantalla. Hilt entra al final: de singletons a mano a inyección.

- **DataStore**: guarda la sesión (token o usuario) fuera de la memoria
- **Pruebas**: la validación se prueba sin interfaz, con casos límite
- **DI**: de crear singletons a mano a pedirlos en el constructor (Hilt)
- Checklist de la práctica: navegación con argumento, estado en ViewModel, Room observado, red con 3 estados y sesión persistida

---

## Resumen exprés de la unidad

Note: Si el alumno recuerda cinco frases, son estas. Repetirlas al final de clase y preguntarlas al día siguiente.

- Navegación: **rutas con argumento tipado**, no pantallas sueltas
- Estado: **ViewModel + StateFlow**, una sola vía de escritura
- Room: **entidad, DAO y Flow**; las consultas se verifican al compilar
- Red: **DTO, interfaz y cliente**; la pantalla solo pinta estados
- Todo se prueba: validación sin UI y migración con datos reales

---

## Qué entra en la práctica

Note: Avisar de esto el primer día de la unidad: es lo que se corrige.

- Navegar a un detalle **pasando un id** y comprobarlo con un id inexistente
- Formulario con **estado en ViewModel** y error mostrado en pantalla
- Lista de Room que **se actualiza sola** al insertar
- Llamada de red con **tres estados visibles** (cargando, error, datos)
- Sesión guardada en **DataStore** que sobrevive al reinicio de la app
