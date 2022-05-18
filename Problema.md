
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
Responde a las siguientes preguntas: 
- ¿Detectas algún problema en el código anterior para realizar el cambio propuesto? Si tu respuesta es correcta, ¿Cuál es el problema?
- Según la introducción en la que se describe el principio de inversión de dependencias ¿Cuál es la clase de alto nivel?
- Según la introducción en la que se describe el principio de inversión de dependencias ¿Cuál es la clase de bajo nivel?
- Si identificas dependencias en el código ¿Qué clase tiene la dependencia?
- Si identificas dependencias en el código ¿De quién depende la clase en la que encuentras la dependencia?
- ¿Cuando se dicen que ambos módulos/clases deben depender de abstracciones, a que elementos de un lenguaje de programación se refiere con abstracciones?
- Si has detectado algún problema en el código anterior. Explica brevemente qué harías para resolverlo.
## 4. Bibliografía