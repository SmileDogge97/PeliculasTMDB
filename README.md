# Examen Salinas - Películas App

Aplicación móvil Android desarrollada en **Kotlin** que permite a los usuarios explorar una lista de películas, filtrarlas y ver sus detalles. La aplicación consume la API de [The Movie Database (TMDB)](https://www.themoviedb.org/).

## 📱 Características Principales

*   **Listado de Películas:** Muestra un catálogo de películas en formato de lista.
*   **Paginación (Infinite Scroll):** Carga automáticamente más películas a medida que el usuario hace scroll hacia abajo en la lista.
*   **Filtros (Ordenamiento):** Permite organizar las películas por:
    *   *Playing Now* (Estrenos recientes).
    *   *Most Popular* (Más populares).
*   **Detalles de Película:** Pantalla dedicada para mostrar información detallada de una película seleccionada, incluyendo póster, título, fecha de estreno, idioma original, descripción y popularidad.
*   **Manejo de Estado de Red:** Valida la conexión a internet antes de realizar peticiones.

## 🛠️ Arquitectura y Tecnologías

El proyecto está estructurado siguiendo los principios de **Clean Architecture** junto con el patrón de diseño **MVVM (Model-View-ViewModel)**, logrando una separación clara de responsabilidades, escalabilidad y facilidad para realizar pruebas.

### Stack Tecnológico

*   **Lenguaje:** [Kotlin](https://kotlinlang.org/)
*   **Arquitectura:** Clean Architecture + MVVM.
*   **Inyección de Dependencias:** [Dagger Hilt](https://dagger.dev/hilt/).
*   **Consumo de API / Red:** [Retrofit2](https://square.github.io/retrofit/) y [OkHttp](https://square.github.io/okhttp/).
*   **Asincronismo:** [Kotlin Coroutines](https://kotlinlang.org/docs/coroutines-overview.html) para el manejo de hilos y llamadas de red.
*   **Componentes de Arquitectura (Jetpack):**
    *   `ViewModel`: Para manejar la lógica de presentación.
    *   `LiveData`: Para observar los cambios de estado en la interfaz de usuario.
    *   `ViewBinding`: Para interactuar de forma segura con los componentes de la vista XML.
*   **Carga de Imágenes:** [Picasso](https://square.github.io/picasso/) para la descarga y renderizado de los pósters de las películas.

## 📂 Estructura del Proyecto

El código fuente está dividido principalmente en las siguientes capas y paquetes:

*   **`data`**: Contiene la implementación de los repositorios y las fuentes de datos (`MoviesDataSource`, `MoviesRepositoryImpl`).
*   **`domain`**: Contiene las interfaces del repositorio y los Casos de Uso (`MoviesUseCase`). Esta capa define las reglas de negocio puras, sin dependencias del framework de Android.
*   **`framework`**: Contiene todo lo relacionado con el SDK de Android y librerías de terceros.
    *   **`data`**: Implementación de los endpoints de Retrofit (`MoviesEndPoint`), modelos de datos (Data Classes como `ResponseList`, `Result`) y la implementación del `DataSource`.
    *   **`di`**: Módulos de Dagger Hilt para la provisión de dependencias (ej. `MoviesModule`).
    *   **`presentation`**: ViewModels (`MoviesViewModel`) y estados de la vista (`MoviesViewState`).
    *   **`ui`**: Actividades (`MainActivity`, `MoviesDetalles`) y adaptadores para RecyclerView (`MoviesListAdapter`).
*   **`utils`**: Clases utilitarias como el manejo de validaciones de red (`ValidarR`), base del caso de uso (`UseCase`) y llaves de API.

## 🚦 Estados de la Vista (State Management)

La interfaz de usuario reacciona a los diferentes estados de la petición de red mediante la clase sellada (Sealed Class) `MoviesViewState`:
*   `Loading`: Muestra un indicador de carga (`ProgressBar`).
*   `Success`: Oculta el indicador y carga los datos en el `RecyclerView`.
*   `Error`: Muestra un mensaje amigable al usuario en caso de fallo.
*   `MoviesNotFound`: Maneja el caso de respuestas vacías.

## 🚀 Cómo ejecutar el proyecto

1.  Clona este repositorio o descarga el código fuente: https://github.com/SmileDogge97/ExamenSalinas.git.
2.  Abre el proyecto en **Android Studio**.
3.  Espera a que Gradle sincronice todas las dependencias (Hilt, Retrofit, Picasso, etc.).
4.  Ejecuta la aplicación en un emulador o en un dispositivo físico.

## 👨‍💻 Buenas Prácticas Aplicadas

*   **Inyección de Dependencias:** Permite que las clases sean más modulares y fáciles de testear.
*   **Desacoplamiento UI/Lógica:** Toda la lógica de negocio y peticiones de red ocurren fuera de las Activities.
*   **Manejo de Errores Seguros:** Uso de `runCatching` en Kotlin Coroutines para atrapar excepciones de red sin que la aplicación se cierre de forma inesperada.
*   **Scroll Infinito Eficiente:** Uso del `OnScrollListener` en el RecyclerView para invocar la siguiente página solo cuando es necesario, mejorando la experiencia del usuario sin sobrecargar la memoria.