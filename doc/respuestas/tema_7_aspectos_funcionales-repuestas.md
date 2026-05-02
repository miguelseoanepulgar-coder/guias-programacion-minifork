<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Un puntero a función en C es una variable que almacena la dirección de una función, permitiendo invocarla indirectamente y tratándola como dato. Esto permite pasar funciones como parámetros o seleccionarlas dinámicamente. En el ejemplo, se define una función que convierte una cadena a mayúsculas y un puntero local aMayusculas que apunta a ella y la invoca.

#include <stdio.h>
#include <ctype.h>

char* convertirMayus(char* s) {
    for (int i = 0; s[i]; i++)
        s[i] = toupper(s[i]);
    return s;
}

int main() {
    char texto[] = "hola";
    char* (*aMayusculas)(char*) = convertirMayus;
    printf("%s\n", aMayusculas(texto));
}



## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Una función lambda es una función anónima tratada como valor, útil para pasar comportamientos sin necesidad de declarar funciones completas. En JavaScript se asigna directamente a una variable, mientras que en Java se usa una interfaz funcional como Function<String,String> para almacenar la lambda.

const aMayusculas = s => s.toUpperCase();
console.log(aMayusculas("hola"));


import java.util.function.Function;

Function<String,String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola"));



## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### El paradigma funcional se basa en funciones puras, ausencia de efectos laterales y tratamiento de funciones como valores. Java 8 es multiparadigma porque combina orientación a objetos con elementos funcionales como lambdas y streams. Las funciones son “ciudadanos de primera clase” cuando pueden almacenarse, pasarse y devolverse igual que cualquier otro dato.


## 4. Explica la sintaxis básica de una función lambda en Java.

### Una lambda en Java sigue la forma (parámetros) -> expresión o (parámetros) -> { bloque }. Si hay un único parámetro, pueden omitirse paréntesis; si el cuerpo es una sola expresión, no se requieren llaves ni return. Su tipo siempre es una interfaz funcional.



## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

function transformar(s, fn) {
    return fn(s);
}
console.log(transformar("hola", s => s.toUpperCase()));


String transformar(String s, Function<String,String> fn) {
    return fn.apply(s);
}
System.out.println(transformar("hola", s -> s.toUpperCase()));



## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

console.log(transformar("hola", s => s.split("").reverse().join("")));


System.out.println(transformar("hola", s -> new StringBuilder(s).reverse().toString()));

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Un closure es una función que captura variables del entorno donde fue definida, permitiendo usarlas incluso después de salir de ese ámbito. En Java, las lambdas pueden acceder a variables locales efectivamente finales. En el ejemplo, la lambda concatena una cadena externa.

String sufijo = "!!!";
Function<String,String> añadirSufijo = s -> s + sufijo;
System.out.println(añadirSufijo.apply("hola"));

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Un puntero a función en C es una dirección de memoria sin información adicional, mientras que una lambda en Java es un objeto que implementa una interfaz funcional, con captura de contexto y comprobación estática de tipos. Las lambdas son más seguras y expresivas.



## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Una función puede devolver otra función, creando comportamientos parametrizados. crearDescuento devuelve una lambda que aplica un porcentaje, capturando dicho valor como closure.

Function<Double,Double> crearDescuento(double pct) {
    return precio -> precio * (1 - pct);
}

var d10 = crearDescuento(0.10);
var d20 = crearDescuento(0.20);

System.out.println(d10.apply(100.0));
System.out.println(d20.apply(100.0));


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Una interfaz funcional es una interfaz con un único método abstracto, usada como tipo para lambdas. Puede tener métodos default o static, pero solo un método abstracto. Se marca opcionalmente con @FunctionalInterface.

@FunctionalInterface
interface Transformador {
    String aplicar(String s);
}



## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

@FunctionalInterface
interface Transformador<T,R> {
    R aplicar(T valor);
}

Transformador<Double,Integer> redondear = d -> (int)Math.round(d);


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Java incluye interfaces funcionales como Function, Consumer, Supplier, Predicate, UnaryOperator, BinaryOperator, entre otras, cubriendo transformaciones, consumo, producción y predicados booleanos.



## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

List<Integer> lista = List.of(-1, 2, 0, 5);
lista.forEach(n -> { if (n > 0) System.out.println(n + " es positivo"); });


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Se usa ? super T para permitir consumidores de tipos más generales, siguiendo PECS: Producer Extends, Consumer Super. Para un método transformar, si la función consume un String, conviene permitir ? super String para mayor flexibilidad


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Las referencias a métodos permiten usar métodos existentes como funciones. En JavaScript se usa directamente la referencia, mientras que en Java se usa obj::método.

class Persona {
  constructor(nombre) { this.nombre = nombre; }
  saludar() { console.log("Hola " + this.nombre); }
}
let p = new Persona("Ana");
let ref = p.saludar.bind(p);
ref();


class Persona {
    String nombre;
    Persona(String n) { nombre = n; }
    void saludar() { System.out.println("Hola " + nombre); }
}

Persona p = new Persona("Ana");
Runnable ref = p::saludar;
ref.run();



## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Java permite referencias a: métodos estáticos, contructores, métodos de instancia concretos, y métodos de instancia sobre cualquier instancia.

Function<String,Integer> f1 = Integer::parseInt;      // estático
Supplier<Persona> f2 = Persona::new;                  // constructor
Persona p = new Persona("Ana");
Runnable f3 = p::saludar;                             // instancia concreta
Function<Persona,String> f4 = Persona::getNombre;     // cualquier instancia

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

Collections.sort(lista, (a,b) -> {
    int cmp = Integer.compare(a.edad, b.edad);
    return cmp != 0 ? cmp : a.nombre.compareTo(b.nombre);
});


lista.sort(
    Comparator.comparingInt(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);

