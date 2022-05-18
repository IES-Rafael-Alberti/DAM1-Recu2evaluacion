
# Prueba especifica recuperacion 2 Eva - 1 DAM

> Se evaluará el RA5, RA6


## 1. Principio de inversión de dependencias

Este principio nos dice que las dependencias deben basarse en abstracciones, no en concreciones. Es decir:

* Los módulos de alto nivel no deben depender de módulos de bajo nivel. Ambos deben depender de abstracciones.
* Las abstracciones no deben depender de detalles. Los detalles deben depender de abstracciones.

Cuando nuestra aplicación esté compuesta por muchos módulos debemos usar inyección de dependencias. Esto nos permitirá tener centralizadas las funcionalidades, evitando tenerlas repartidas por todo el código. Además, esto nos facilitará realizar pruebas en el código.

Imaginemos que tenemos una clase para acceder a datos `ServicioBaseDatos` que lo realiza a través de una BBDD, y una clase `GestionNegocio` que usa ese servicio.

```Kotlin

class ServicioBaseDatos {

  /** simula una BD mediante un map*/
  private val bd = mutableMapOf<Int, String>(
    1 to "uno",
    33 to "treinta y tres",
    0 to "cero",
    44 to "cuarenta y cuatro"
  )

  fun obtenerDatoById(id:Int): Dato? {
    //...
  }

  fun obtenerDatos(): List<Dato>  {
    //...
  }

  fun eliminaDato(id:Int): Boolean {
    //...
  }

  fun enviarDatos(dato: Dato): Boolean {
    //...
  }

}

class GestionNegocio {

  /**
   * Servicio de acceso a datos
   */
  private val servicioAccesoDatos = ServicioBaseDatos()
  //private val servicioAccesoDatosBD = ServicioAccesoDatosAPI()

  /**
   * Obtener el dato identificado por id
   *
   * @param id El identificador del dato a obtener
   * @return El dato obtenido, null si no lo encuentra
   */
  fun obtenerDatoById(id:Int): Dato? {
    return servicioAccesoDatos.obtenerDatoById(id);
  }

  /**
   * Obtiene todos los datos.
   *
   * @return La lista de todos los datos. Lista vacía si no existe nada.
   */
  fun obtenerDatos(): List<Dato> {
    return servicioAccesoDatos.obtenerDatos();
  }
  
  /**
   * Elimina dato identificado por id
   *
   * @param id Identificador del datos a eliminar
   * @return true si existia y se elimino. False en caso contrario
   */
  fun eliminaDato(id:Int): Boolean {
    return servicioAccesoDatos.eliminaDato(id)
  }

  /**
   * Enviar datos, inserta si no existe, si existe lo actualiza.
   *
   * @param dato el dato a enviar para persistir
   * @return true si fue una inserción, false si fue una actualización
   */
  fun enviarDatos(dato:Dato) : Boolean {
    return servicioAccesoDatos.enviarDatos(dato)
  }
}

/**
 * Main Prueba del código
 *
 * @param args
 */
fun main(args: Array<String>) {
    val gestionNegocio = ....
    gestionNegocio.enviarDatos(Dato(50, "Cincuenta"))
    gestionNegocio.enviarDatos(Dato(50, "Cincuenta y uno"))
    gestionNegocio.eliminaDato(0)
    println(gestionNegocio.obtenerDatos())
}
```

Más adelante queremos **_cambiar_** a otro **servicio de acceso a datos**, que en vez obtener los datos por BBDD lo hace consultando una API.


## 2. ¿Qué se pide?

1. Implementar el código necesario para desarrollar el `ServicioBaseDatos`
2. Implementar el código necesario para desarrollar el servicio que consulta los datos a través una API. Este servicio, simulará la consulta a la API a través de un Set  `private val bd = mutableSetOf<Dato>`
3. Refactorizar el código para cumplir con el principio de inversión de dependencias en la clase `GestionNegocio`.
4. Dejar el main listo para probar con los dos servicios. La información mostrada tiene que ser legible.

## 3. Ejecución y test
Para probar la práctica se utilizarán la clase main, en la que se probarán los siguientes métodos, haciendo uso de los dos servicios:
- `obtenerDatoById()`: Obtener dato por id 
- `enviarDatos()`: Inserción y actualización de dato
- `eliminaDato()`: Eliminar dato
- `obtenerDatos()`: Obtener todos los datos

## 4. Evaluación

#### Desarrolla programas organizados en clases analizando y aplicando los principios de la programación orientada a objetos.
 
0. No lo hace; 5. Intenta pero no aplica correctamente el PID, aunque funcional. 10. Aplica correctamente el principo de PID, de forma elegante.

#### Desarrolla programas aplicando características avanzadas de los lenguajes orientados a objetos y del entorno de programación.

0. No lo hace; 5. Funciona menos de la mitad de los métodos. 10. Funciona más de la mitad de los métodos.

Adicionalmente se tendrá en cuenta:
- El uso de clases, y de jerarquía de clases propias si son necesarias o ya conocidas y que nos las proporcionan kotlin, como por ejemplo List, Map, Set. etc.
- El código realizado es óptimo.
- El código realizado es limpio y está comentado.
- Se cumple requisitos de entrega.

## 5. Condiciones de entrega

Se entrega la URL al repositorio.
> **IMPORATANTE!!!** La no compilación del código, supone la perdida del derecho de corregir el examen.

## 4. Bibliografía