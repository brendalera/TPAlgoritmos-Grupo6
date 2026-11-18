# Diagrama de clases UML — Sistema de Bicicletas Públicas

Trabajo Práctico de Algoritmos — UNAHUR (grupo 6)

> El diagrama está escrito en [Mermaid](https://mermaid.js.org/), que GitHub renderiza automáticamente en archivos `.md`.
> Para verlo como imagen también se puede pegar el bloque en <https://mermaid.live>.

## Convenciones

| Símbolo | Significado |
|---|---|
| `A *-- B` | **Composición**: A contiene a B y lo crea y destruye (B no existe sin A) |
| `A --> B` | **Asociación**: A conoce o referencia a B, pero no es su dueño |
| `A ..> B` | **Dependencia**: A usa a B de forma puntual (por ejemplo, dentro de un método) |
| `+` / `-` | Miembro público / privado |
| `Tipo[]` | Arreglo dinámico o puntero a arreglo (`Tipo*` en el código) |

## Diagrama completo

```mermaid
classDiagram
direction TB

class main {
    +cargarEstaciones(ruta, TablaHash, Grafo) bool
    +cargarCiclovias(ruta, Grafo) bool
    +cargarBicicletas(ruta, ArbolBicicletas) bool
    +cargarViajes(ruta, ColeccionViajes) bool
    +mostrarMenu()
    +main() int
}

%% ---------- Estructuras principales ----------
class TablaHash {
    -int tamanio = 101
    -NodoHash[] tabla
    -int cantidad
    -int colisiones
    -funcionHash(string codigo) int
    +insertar(Estacion) bool
    +buscar(string codigo, Estacion resultado) bool
    +eliminar(string codigo) bool
    +obtener(string codigo) bool
    +getCantidad() int
    +getColisiones() int
    +factorCarga() double
    +mostrarTodas()
    +copiarEnArreglo(int cantidadCopiada) Estacion[]
}

class MinHeap {
    -ItemHeap[] items
    -int cantidad
    -int capacidad
    -int proximoOrden
    -esMasUrgente(ItemHeap a, ItemHeap b) bool
    -intercambiar(int i, int j)
    -crecer()
    -subir(int posicion)
    -bajar(int posicion)
    +insertar(Bicicleta, int prioridad) bool
    +extraerMasUrgente(Bicicleta, int prioridad) bool
    +contiene(int id) bool
    +estaVacio() bool
    +getCantidad() int
}

class Grafo {
    -string[] codigos
    -bool[] activas
    -Arista[] adyacencia
    -int cantidad
    -int capacidad
    -indiceDe(string codigo) int
    -crecer()
    -existeArista(int a, int b) bool
    -liberarAristas(int nodo)
    -quitarArista(int nodo, int destino)
    +existeEstacion(string codigo) bool
    +agregarEstacion(string codigo) bool
    +agregarCiclovia(string a, string b) bool
    +eliminarEstacion(string codigo) bool
    +caminoMasCorto(string origen, string destino) int
}

class ArbolBicicletas {
    -NodoArbol raiz
    -int cantidad
    -insertar(NodoArbol, Bicicleta, bool) NodoArbol
    -eliminar(NodoArbol, int id, bool) NodoArbol
    -inorder(NodoArbol)
    -contarAveriadas(NodoArbol) int
    -altura(NodoArbol) int
    -listarMasDeKm(NodoArbol, double km) int
    -liberar(NodoArbol)
    +insertar(Bicicleta) bool
    +buscar(int id) Bicicleta
    +eliminar(int id) bool
    +listarInorder()
    +contarAveriadas() int
    +altura() int
    +listarMasDeKm(double km) int
    +getCantidad() int
}

class GestorEspera {
    -string[] codigo
    -ColaEspera[] colas
    -int cantidad
    -int capacidad
    -indiceDe(string codigo) int
    +crecer()
    +obtenerCola(string codigo) ColaEspera
}

class ColeccionViajes {
    -Viaje[] viajes
    -int cantidad
    -int capacidad
    -crecer()
    +agregar(Viaje)
    +getCantidad() int
    +contarViajesDesdeEstacion(string estacion, string desde, string hasta) int
}

class Redistribucion {
    -string[] codigo
    -int[] faltantes
    -bool[] elegidas
    -int cantidad
    -int capacidad
    -int capacidadCamion
    -int soluciones
    -crecer()
    -buscar(int indice, int sumaActual)
    -mostrarSolucion()
    +agregarEstacion(string codigo, int faltante) bool
    +resolver(int capacidadCamion) int
    +getCantidad() int
}

class OrdenamientoEstaciones {
    +claveDeOrden(Estacion, bool porCapacidad) int
    +ordenarEstaciones(Estacion[] arreglo, int cantidad, bool porCapacidad)
}

%% ---------- Colas ----------
class ColaEspera {
    -NodoCola frente
    -NodoCola fin
    -int cantidad
    +encolar(string dni) int
    +desencolar(string dni) bool
    +estaVacia() bool
    +getCantidad() int
    +mostrar()
}

%% ---------- Nodos internos ----------
class NodoHash {
    +Estacion estacion
    +NodoHash siguiente
}

class ItemHeap {
    +Bicicleta bicicleta
    +int prioridad
    +int orden
}

class Arista {
    +int destino
    +Arista siguiente
}

class NodoArbol {
    +Bicicleta bicicleta
    +NodoArbol izquierdo
    +NodoArbol derecho
}

class NodoCola {
    +string dni
    +NodoCola siguiente
}

%% ---------- Entidades ----------
class Estacion {
    -string codigo
    -string nombre
    -string barrio
    -int capacidadAnclaje
    -int bicisDisponibles
    +getCodigo() string
    +getNombre() string
    +getBarrio() string
    +getCapacidadAnclajes() int
    +getBicisDisponibles() int
    +setNombre(string)
    +setBarrio(string)
    +setCapacidadAnclajes(int)
    +setBicisDisponibles(int)
    +estaBajaOcupacion() bool
    +mostrar()
}

class Bicicleta {
    -int id
    -string codigoEstacion
    -string tipo
    -string estado
    -double kilometros
    +getId() int
    +getCodigoEstacion() string
    +getTipo() string
    +getEstado() string
    +getKilometros() double
    +setCodigoEstacion(string)
    +setTipo(string)
    +setEstado(string)
    +setKilometros(double)
    +estaAveriada() bool
    +mostrar()
}

class Viaje {
    -int id
    -int biciId
    -string dniUsuario
    -string estacionOrigen
    -string estacionDestino
    -string fecha
    -int duracionMin
    +getId() int
    +getBicicletaId() int
    +getDniUsuario() string
    +getEstacionOrigen() string
    +getEstacionDestino() string
    +getFecha() string
    +getDuracionMin() int
    +setEstacionOrigen(string)
    +setEstacionDestino(string)
    +setDuracionMin(int)
    +mostrar()
}

%% ---------- Composición: main contiene las estructuras ----------
main *-- TablaHash
main *-- MinHeap
main *-- Grafo
main *-- ArbolBicicletas
main *-- GestorEspera
main *-- ColeccionViajes

%% ---------- Composición: estructura contiene sus nodos ----------
TablaHash "1" *-- "0..*" NodoHash
NodoHash *-- Estacion
MinHeap "1" *-- "0..*" ItemHeap
ItemHeap *-- Bicicleta
Grafo "1" *-- "0..*" Arista
ArbolBicicletas "1" *-- "0..*" NodoArbol
NodoArbol *-- Bicicleta
GestorEspera "1" *-- "0..*" ColaEspera
ColaEspera "1" *-- "0..*" NodoCola
ColeccionViajes "1" *-- "0..*" Viaje

%% ---------- Asociaciones: enlaces entre nodos ----------
NodoHash --> NodoHash : siguiente
Arista --> Arista : siguiente
NodoArbol --> NodoArbol : izquierdo / derecho
NodoCola --> NodoCola : siguiente

%% ---------- Asociaciones entre entidades (por código o id) ----------
Bicicleta --> Estacion : está en
Viaje --> Bicicleta : usa
Viaje --> Estacion : origen / destino

%% ---------- Dependencias ----------
main ..> Redistribucion : crea en la opción 20
main ..> OrdenamientoEstaciones : usa en la opción 11
OrdenamientoEstaciones ..> Estacion : ordena
```

## Resumen de relaciones

### Composición

| Todo | Parte | Por qué |
|---|---|---|
| `main` | `TablaHash`, `MinHeap`, `Grafo`, `ArbolBicicletas`, `GestorEspera`, `ColeccionViajes` | `main` crea las estructuras y vive mientras corre el programa |
| `TablaHash` | `NodoHash` | Cada posición de la tabla tiene una lista enlazada de nodos |
| `NodoHash` | `Estacion` | El nodo guarda la estación por valor |
| `MinHeap` | `ItemHeap` | El arreglo del heap está formado por ítems |
| `ItemHeap` | `Bicicleta` | El ítem guarda la bicicleta con su prioridad |
| `Grafo` | `Arista` | Lista de adyacencia: cada nodo tiene una lista de aristas |
| `ArbolBicicletas` | `NodoArbol` | El árbol es un conjunto de nodos enlazados |
| `NodoArbol` | `Bicicleta` | Cada nodo guarda una bicicleta |
| `GestorEspera` | `ColaEspera` | Una cola por estación |
| `ColaEspera` | `NodoCola` | La cola es una lista enlazada |
| `ColeccionViajes` | `Viaje` | Arreglo dinámico de viajes |

### Asociación

| Origen | Destino | Significado |
|---|---|---|
| `NodoHash`, `Arista`, `NodoCola` | sí mismos (`siguiente`) | Enlace al próximo nodo de la lista |
| `NodoArbol` | `NodoArbol` | Hijo izquierdo y derecho |
| `Bicicleta` | `Estacion` | Referencia a la estación por `codigoEstacion` |
| `Viaje` | `Bicicleta` | Referencia a la bici por `biciId` |
| `Viaje` | `Estacion` | Referencia a origen y destino por código |

### Dependencia

| Origen | Destino | Significado |
|---|---|---|
| `main` | `Redistribucion` | Se instancia localmente al elegir la opción 20 del menú |
| `main` | `OrdenamientoEstaciones` | Se usa para listar estaciones ordenadas (opción 11) |
| `OrdenamientoEstaciones` | `Estacion` | Ordena un arreglo de estaciones |

## Notas sobre el diagrama

- La clase del heap se llama `MinHeap` en el código (en la imagen original figuraba como `MiniHeap`).
- El diagrama original no tenía relaciones entre clases fuera de `main`; se agregaron las composiciones y asociaciones que pide el enunciado.
- Los métodos privados auxiliares (`crecer`, `subir`, `bajar`, etc.) se mantienen del diagrama original. Si el código cambia, conviene actualizar este archivo.