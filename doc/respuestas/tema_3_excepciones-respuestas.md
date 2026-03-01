<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### 1.La función devuelve int (0 = éxito, no 0 = error) y coloca el resultado en un puntero. Quien llama decide el mensaje al usuario según el código de retorno. 

#include <stdio.h>
#include <math.h>

int raiz(double x, double *out) {
    if (x < 0.0 || out == NULL) return -1;  // error de dominio o argumento nulo
    *out = sqrt(x);
    return 0; // ok
}

int main(void) {
    double x = -9.0, r;
    int rc = raiz(x, &r);
    if (rc != 0) {
        fprintf(stderr, "Error: la entrada debe ser no negativa (x=%.2f)\n", x);
    } else {
        printf("raiz(%.2f) = %.4f\n", x, r);
    }
    return 0;
}
###2.La función devuelve el resultado directamente; si hay error, establece errno (por ejemplo, EDOM para error de dominio) y retorna NAN como centinela.


#include <stdio.h>
#include <errno.h>
#include <math.h>
#include <fenv.h>

double raiz_errno(double x) {
    if (x < 0.0) {
        errno = EDOM;      // error de dominio
        return NAN;        // centinela
    }
    errno = 0;             // (opcional) no imprescindible si el llamador limpia
    return sqrt(x);
}

int main(void) {
    double x = -9.0;
    errno = 0;             // limpiar antes de la llamada
    double r = raiz_errno(x);
    if (errno == EDOM || isnan(r)) {
        fprintf(stderr, "Error: argumento negativo (x=%.2f)\n", x);
    } else {
        printf("raiz(%.2f) = %.4f\n", x, r);
    }
    return 0;
}



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Una excepción es un mecanismo mediante el cual un lenguaje de programación señala que se ha producido una situación anómala durante la ejecución normal de un programa. En lugar de continuar con el flujo normal, se para el programa y se señala el posible error.Para quien llama a funciones, el uso de excepciones permite concentrar el manejo de errores en bloques específicos (try–catch en Java), lo que mejora la legibilidad y evita olvidar verificar valores de retorno. 


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### 
public class Calculadora {

    // Lanza una excepción si x < 0; de lo contrario, devuelve la raíz cuadrada
    public static double raiz(double x) {
        if (x < 0.0) {
            throw new IllegalArgumentException("El argumento debe ser no negativo: x=" + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        double x1 = 9.0;
        double x2 = -9.0;

        // Llamada correcta
        try {
            double r1 = Calculadora.raiz(x1);
            System.out.printf("raiz(%.2f) = %.4f%n", x1, r1);
        } catch (IllegalArgumentException e) {
            System.err.println("Error al calcular la raíz: " + e.getMessage());
        }

        // Llamada que provoca error y se controla desde fuera
        try {
            double r2 = Calculadora.raiz(x2);
            System.out.printf("raiz(%.2f) = %.4f%n", x2, r2);
        } catch (IllegalArgumentException e) {
            System.err.println("Error al calcular la raíz: " + e.getMessage());
            // Aquí podría registrarse el error, pedir otro dato, etc.
        }
    }


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Una excepción se lanza cuando un método detecta una situación anómala que impide continuar con su ejecución normal. “Lanzar” significa crear un objeto que representa el error y interrumpir inmediatamente el flujo, abandonando el método en ese punto.“Controlar” o capturar una excepción consiste en rodear con un bloque try el código que podría fallar y escribir uno o varios catch que indiquen qué tipos de excepción se desean manejar. Si el cálculo falla, el flujo salta automáticamente al catch correspondiente. La propagación ocurre cuando, tras lanzarse la excepción, no existe en ese método un catch adecuado. La JVM comienza entonces a “deshacer la pila de llamadas” (stack unwinding): abandona el método actual, destruye su marco de pila, y sube al método que lo llamó. Si ese método tampoco tiene un catch compatible, también se abandona y se continúa subiendo. Este proceso continúa hasta encontrar un catch apropiado o, si no lo hay, hasta terminar el programa con un mensaje de error. Las funciones por las que pasa la excepción no se reanudan en ningún punto: se abandonan definitivamente, como si hubieran terminado abruptamente.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### La propagación natural de excepciones en Java aporta varias ventajas significativas frente al modelo clásico de C basado en códigos de error. En primer lugar, permite separar el flujo normal del flujo de errores, evitando la necesidad de comprobar manualmente cada valor de retorno tras cada llamada, algo frecuente en C.Otra ventaja es que la propagación automática realiza un abandono ordenado de los métodos (stack unwinding), garantizando que no se continúe ejecutando código en estados inconsistentes.Además, la propagación facilita que cada nivel del programa decida si sabe manejar el error o debe delegarlo hacia niveles superiores.Por último, la integración de la propagación con mecanismos como finally y try-with-resources reduce la posibilidad de filtraciones de recursos.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### En la mayoría de lenguajes orientados a objetos, como Java, las excepciones son objetos que pertenecen a una jerarquía de clases.Desde el punto de vista de la encapsulación, este diseño permite ocultar los detalles internos del error y exponer únicamente la información necesaria mediante métodos públicos.Al tratarse de objetos, es completamente posible crear excepciones personalizadas, extendiendo clases como Exception o RuntimeException.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Una excepción en Java encapsula información esencial sobre el error, algo que en C no está disponible de forma automática. El dato más importante que lleva consigo cualquier objeto excepción es el stack trace: la traza completa de métodos que estaban en ejecución en el momento de producirse el fallo. Esta traza permite saber exactamente dónde ocurrió el error y qué camino siguió el programa hasta llegar allí.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Sí, en Java es posible tener más de un bloque catch asociado a un mismo try. Cada bloque catch especifica un tipo distinto de excepción a manejar, lo que permite reaccionar de forma diferente según el error concreto que se haya producido.Cuando ocurre una excepción dentro del try, solo se ejecuta un único catch: concretamente, el primer catch cuyo tipo sea compatible con el tipo de la excepción lanzada.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### La garantía de ejecutar siempre un código de limpieza en Java se obtiene con el bloque finally. El finally siempre se ejecuta tras el try (y tras el catch si lo hay), tanto si todo va bien como si se lanza y se propaga una excepción, o incluso si hay return dentro del try/catch


import java.io.*;

public class EjemploFinallyConCatch {
    public static void main(String[] args) {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine(); // puede lanzar IOException
            System.out.println("Primera línea: " + linea);
        } catch (IOException e) {
            System.err.println("Error de E/S: " + e.getMessage());
        } finally {
            // Se ejecuta SIEMPRE: éxito, error, o return previo
            if (br != null) {
                try { br.close(); } catch (IOException e) {
                    System.err.println("Error al cerrar: " + e.getMessage());
                }
            }
        }
    }
}


import java.io.*;

public class EjemploFinallySinCatch {
    // Propaga la IOException; aquí solo se garantiza el cierre
    static void procesa() throws IOException {
        BufferedReader br = null;
        try {
            br = new BufferedReader(new FileReader("datos.txt"));
            String linea = br.readLine(); // si falla, saltará al finally
            System.out.println("Primera línea: " + linea);
        } finally {
            // Cierre garantizado incluso si se lanzó IOException
            if (br != null) {
                try { br.close(); } catch (IOException e) {
                    System.err.println("Error al cerrar: " + e.getMessage());
                }
            }
        }
    }

    public static void main(String[] args) {
        try {
            procesa();
        } catch (IOException e) {
            System.err.println("Fallo al procesar: " + e.getMessage());
        }
    }
}



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Sí, en Java un bloque finally puede ir sin catch, siempre que exista un try delante.El bloque finally se ejecuta siempre, tanto si ocurre como si no ocurre una excepción dentro del try, incluso cuando existe un return dentro del try, el finally también se ejecuta antes de que el método termine definitivamente. El return queda “pendiente” hasta que el finally se haya completado.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

