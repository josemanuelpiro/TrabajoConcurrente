# Trabajo Concurrente

Proyecto dirigido a la confeccion de un Monitor de concurrencia basado en redes de Petri.

## Caracteristicas del Monitor

Monitor utilizado para el manejo y sincronizaron de hilos con el fin de simular un sistema a través de una red de Petri, aplicando alguna de las propiedades de esta.

## Descripcion y funcionamiento

- El monitor inicializara todas las variables necesarias para su ejecución. Las características de la red
de Petri y del sistema son leídas desde un archivo JSON, lo cual nos da la posibilidad de cambiar la red con la que trabaja el monitor,
los tiempos, etc.

- El monitor se encuentra dividido en 4 paquetes separados para independizar el funcionamiento de cada uno.

### RDP

- Aqui se encuentra todo lo necesario para la ejecución y evolución correcta de una red de Petri con o sin tiempo. Esta red se
construye a través de un archivo JSON.

### Queue

- Clase encargada de administrar los hilos que se encuentren durmiendo o esperando a que alguien los despierte para continuar
su ejecución.

### Policy

- En la clase policy se encuentran las herramientas para resolver los conflictos que se presenten dentro de la red. Se le proporcionara
los conflictos que hay en la red y quienes están esperando en las colas. Esta nos devolverá a quien debemos despertar y asi continuar
la ejecución de la red. La política implementada es variable y no es posible cambiarla por ahora, pero podrá ser cargada en JSON en un futuro.

### Logger

- En esta pequeña clase se encuentran los recursos para registrar todos los eventos que se producen en la red y en las colas.
Como por ejemplo, cuando un hilo se va a dormir, cuando se levanta, cuando un disparo es exitoso o no, etc.

### Estructura JSON

N: Cantidad de transiciones
M: Cantidad de plazas

#### Campos obligatorios del JSON

- Matriz de incidencia I *[dim: MxN]*
- Vector de marcado *[dim: M]*
- Matriz y vector de invariantes de plaza
  - Deben ser ambos utilizados o ninguno, funcionan en conjunto
  - Matriz de invariantes de plaza. *[dim: IPxM]*
  - Cada fila representa un conjunto de de plazas que formar el invariante (IP Invariantes)
  - Cada columna representa con un 1 si forma parte de algun invariante o 0 si no pertenece
  - El la dimension de la fila es la cantidad de plazas M
- Matriz de ventana temporal *[dim: 2xN]*
  - Cada columna es asignada a una transición.
  - La primera fila es el inicio de la ventana temporal.
  - la otra fila representa el fin de la ventana temporal.
- Vector de Timestamp *[dim: N]*
  - Cada elemento representa una transición.
  - Valor predeterminado a cargar sera -1, la propia red se encargara de actualizar
  - los Timestamp en caso de que la transición se encuentre sensibilizada por tokens.
- Info
  - Simple string de información de la red.

### Invariantes de transición

- **Expresión Regular**

      (T0),(?:.)*?(?:(?:(T1),(?:.)*?(T3),(?:.)*?(?:(?:(T13),(?:.)*?(T7),(?:.)*?)|(?:(T5),(?:.)*?))(?:(?:(T9),(?:.)*?(T15),)|(?:(T10),(?:.)*?(T16))))|(?:(T2),(?:.)*?(T4),(?:.)*?(?:(?:(T14),(?:.)*?(T8),(?:.)*?)|(?:(T6),(?:.)*?))(?:(?:(T11),(?:.)*?(T15),)|(?:(T12),(?:.)*?(T16),))))
