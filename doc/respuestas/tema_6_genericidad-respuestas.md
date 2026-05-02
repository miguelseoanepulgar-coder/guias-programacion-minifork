<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.


public class ArrayGenerico {
    private Object[] datos = new Object[10];
    private int size = 0;

    public void insertar(Object o) {
        datos[size++] = o;
    }

    public Object obtener(int i) {
        return datos[i];
    }

    public static void main(String[] args) {
        ArrayGenerico arr = new ArrayGenerico();

        arr.insertar(42);      // autoboxing a Integer
        arr.insertar(3.14);    // autoboxing a Double

        int x = (Integer) arr.obtener(0);
        double y = (Double) arr.obtener(1);

        System.out.println("Entero: " + x);
        System.out.println("Double: " + y);
    }
}


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### La programación genérica se entiende como un estilo de programación en el que clases, interfaces o métodos se definen de forma parametrizada por tipos. En lugar de fijar de antemano el tipo concreto con el que trabajará una estructura de datos, se permite que dicho tipo sea un parámetro. El ejemplo anterior no constituye programación genérica en sentido estricto. En ambos casos se trata de programación no tipada o tipado débil, donde se acepta cualquier tipo a costa de perder seguridad y depender de castings manuales. Aunque conceptualmente se busca flexibilidad, no existe parametrización de tipos ni comprobación estática, por lo que no se considera genericidad.



## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Al emplear void* en C o Object en Java para construir estructuras de datos “genéricas”, el primer problema aparece en el chequeo de tipos en tiempo de compilación. El compilador no puede verificar si el valor insertado en la estructura coincide con el tipo que el programador espera recuperar después. Esto implica que cualquier error de tipo pasa inadvertido hasta el momento de la ejecución, donde puede manifestarse como un segmentation fault en C o una ClassCastException en Java. La ausencia de comprobación estática convierte estos errores en más difíciles de detectar y depurar.Un segundo problema es la dependencia obligatoria de castings explícitos. Tanto en C como en Java, al recuperar un elemento almacenado como void* u Object, es necesario convertirlo manualmente al tipo original. Si el programador se equivoca en el casting, el error no se detecta hasta que el programa intenta usar el valor. En C, el compilador permite cualquier conversión entre punteros, lo que puede llevar a accesos inválidos a memoria; en Java, aunque el sistema de tipos es más seguro, el error sigue apareciendo en tiempo de ejecución. Además, este enfoque introduce fragilidad en el diseño. La estructura de datos no puede expresar qué tipo debería contener, por lo que no existe documentación implícita ni restricciones formales. Cualquier parte del programa puede insertar valores de tipos incompatibles, generando estructuras heterogéneas difíciles de manejar. Esto contrasta con la genericidad real, donde el tipo se parametriza y queda fijado al instanciar la estructura, garantizando coherencia.Finalmente, se pierde la posibilidad de que el compilador optimice o verifique el uso correcto de la estructura. En C, el uso de void* impide conocer el tamaño real del objeto apuntado, lo que obliga a confiar en el programador para gestionar memoria y alineación. En Java, el uso de Object impide aprovechar la información de tipos para evitar castings o para aplicar transformaciones seguras. En conjunto, estos problemas justifican la introducción de la genericidad como mecanismo para combinar flexibilidad y seguridad de tipos.



## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Los parámetros de tipo son símbolos que representan tipos y que se introducen en la definición de clases, interfaces o métodos para que dichos elementos puedan trabajar de forma genérica con distintos tipos sin perder seguridad de tipos. En lugar de fijar un tipo concreto —por ejemplo, String o Integer— se declara un parámetro como T, E o K, que actuará como un marcador de posición. Cuando se instancia la clase o se invoca el método, ese parámetro se sustituye por un tipo real, permitiendo reutilizar la misma definición para múltiples situaciones sin necesidad de duplicar código.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### En Java, la genericidad permite declarar una lista que solo acepte objetos de tipo String. El compilador garantiza que no se insertarán elementos de otro tipo, eliminando la necesidad de castings y evitando errores en tiempo de ejecución. 

import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        lista.add("Genéricos");

        for (String s : lista) {
            System.out.println("Elemento: " + s.toUpperCase());
        }
    }
}



## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Cuando se instancia una clase con parámetros de tipo, el compilador debe decidir cómo manejar esos tipos genéricos. En Java, el proceso consiste en comprobar estáticamente que el uso de los tipos es correcto y, una vez verificado, eliminar la información de los parámetros de tipo antes de generar el bytecode. Esto significa que en tiempo de ejecución la clase genérica se comporta como si trabajara con Object (o con el límite acotado si lo hubiera). Este comportamiento distinto se debe a decisiones de diseño. Java mantiene compatibilidad hacia atrás con versiones anteriores del lenguaje, donde no existían genéricos, y por eso opta por un mecanismo llamado type erasure. En este proceso, los parámetros de tipo (<T>, <E>, etc.) se eliminan y se sustituyen por Object o por el límite superior indicado (extends). Como consecuencia, en tiempo de ejecución no existe información sobre el tipo genérico, lo que explica restricciones como la imposibilidad de crear new T[] o de usar instanceof con tipos parametrizados. Aun así, la seguridad de tipos se garantiza en tiempo de compilación.




## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

public class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}


public static Par<Double, Double> calcularEstadisticas(double[] datos) {
    double suma = 0;
    for (double d : datos) {
        suma += d;
    }
    double media = suma / datos.length;

    double sumaCuadrados = 0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }
    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}

public static void main(String[] args) {
    double[] valores = {1.0, 2.0, 3.0, 4.0};

    Par<Double, Double> resultado = calcularEstadisticas(valores);

    System.out.println("Media: " + resultado.getPrimero());
    System.out.println("Desviación típica: " + resultado.getSegundo());
}



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}

public static void main(String[] args) {
    Object o = seleccionaUno("Hola", 123);  // mezcla de tipos permitida

    String s = (String) o;  // posible ClassCastException
    System.out.println(s);
}


public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}

public static void main(String[] args) {
    String s = seleccionaUno("Hola", "Mundo");  // ambos String
    System.out.println(s.toUpperCase());

    // seleccionaUno("Hola", 123);  // error de compilación
}



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Sí, en Java es posible restringir los parámetros de tipo mediante bounded type parameters. Esto permite indicar que un tipo genérico <T> debe ser, como mínimo, un subtipo de una clase o interfaz concreta. En el caso de números, Java no permite usar tipos primitivos como límites, pero sí permite usar la clase abstracta Number, de la que derivan Integer, Double, Float, etc. 

public class Punto {
    private final Number x;
    private final Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}


public class Punto<T extends Number> {
    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}


Punto<Integer> p1 = new Punto<>(1, 2);
Punto<Integer> p2 = new Punto<>(4, 6);

double d = p1.calcularDistanciaA(p2);
System.out.println("Distancia: " + d);


Punto<Integer> p3 = new Punto<>(1, 2);
Punto<Double> p4 = new Punto<>(1.0, 2.0);

p3.calcularDistanciaA(p4);  // error de compilación



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### En la solución sin genéricos, donde las coordenadas son simplemente de tipo Number, el compilador permite mezclar libremente distintos subtipos numéricos. Esto significa que es perfectamente válido crear un Punto con una coordenada Integer y otra Double, ya que ambas encajan en el tipo declarado. En cambio, en la solución con genéricos <T extends Number>, el parámetro de tipo obliga a que ambas coordenadas sean del mismo subtipo de Number. Si se declara Punto<Integer>, tanto x como y deben ser Integer; si se declara Punto<Double>, ambas deben ser Double. Respecto al tipo devuelto por los getters, en la versión sin genéricos, getX() devuelve siempre un Number, independientemente del tipo concreto almacenado. Esto obliga a convertir manualmente el valor si se necesita un subtipo específico, como intValue() o doubleValue(). En la versión genérica, en cambio, getX() devuelve un T, es decir, el tipo exacto con el que se instanció el Punto. Si el punto es Punto<Integer>, el getter devuelve un Integer; si es Punto<Float>, devuelve un Float, y así sucesivamente. Finalmente, debido al type erasure, ambas soluciones terminan compilándose a una clase que internamente almacena coordenadas como Number. Sin embargo, la versión genérica aporta una capa adicional de seguridad en tiempo de compilación que evita errores y hace el código más expresivo y robusto.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```



public interface Punto<T extends Punto<T>> {
    double distanciaA(T otro);
}


public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}


public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        double dz = this.z - otro.z;
        return Math.sqrt(dx*dx + dy*dy + dz*dz);
    }
}


Punto2D a = new Punto2D(1, 2);
Punto2D b = new Punto2D(4, 6);
double d = a.distanciaA(b);   //  válido

Punto3D c = new Punto3D(1, 2, 3);
// a.distanciaA(c);           // error de compilación



## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### En Java, aunque String es subtipo de Object, List<String> no es subtipo de List<Object>. Los tipos genéricos en Java son invariantes, lo que significa que List<A> y List<B> no tienen relación de herencia aunque A sea subtipo de B. Esto se debe a que permitir esa relación rompería la seguridad de tipos en tiempo de compilación. En cambio, los arrays en Java sí son covariantes, por lo que String[] sí es subtipo de Object[]. Esta diferencia histórica provoca que los arrays puedan generar errores en tiempo de ejecución, mientras que los genéricos los detectan en compilación.El problema con los arrays aparece cuando se aprovecha esa covarianza para insertar un elemento incompatible. Por ejemplo, si se asigna un String[] a una referencia Object[], el compilador lo permite, pero en tiempo de ejecución se puede intentar insertar un Integer, lo que provoca una ArrayStoreException. Este tipo de error demuestra que la covarianza de arrays es insegura, pero se mantiene por compatibilidad con versiones antiguas del lenguaje. Los genéricos, en cambio, fueron diseñados desde el principio para evitar este tipo de problemas, por lo que su invariancia impide que se produzcan inserciones incorrectas. A partir de estos ejemplos, se puede definir que un tipo genérico es covariante cuando A subtipo de B implica que Gen<A> es subtipo de Gen<B>. Esto permite leer valores con seguridad, pero impide escribir en la estructura. Es contravariante cuando A subtipo de B implica que Gen<B> es subtipo de Gen<A>, lo que permite escribir valores pero limita las lecturas. Finalmente, es invariante cuando no existe relación de herencia entre Gen<A> y Gen<B> aunque A y B sí la tengan, lo que garantiza la máxima seguridad de tipos. Java aplica invariancia por defecto en sus genéricos precisamente para evitar los problemas que sí aparecen con la covarianza de los arrays.



## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Un wildcard (?) es un marcador que representa un tipo desconocido dentro de un tipo genérico. Permite recuperar relaciones de covarianza o contravarianza que los genéricos no ofrecen por defecto. List<? extends T> representa una lista cuyos elementos son de un tipo desconocido que hereda de T. Es una forma de covarianza, útil cuando se quiere leer elementos con seguridad, pero no se quiere permitir añadir nuevos elementos (salvo null). Por el contrario, List<? super T> representa una lista cuyos elementos son de un tipo desconocido que es superclase de T, lo que permite añadir valores de tipo T con seguridad, pero limita las lecturas a Object. 

public static double sumarLista(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}


List<Integer> enteros = List.of(1, 2, 3);
List<Double> reales = List.of(1.5, 2.5);

System.out.println(sumarLista(enteros)); // válido
System.out.println(sumarLista(reales));  // válido



public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(10);
    lista.add(20);
    lista.add(30);
}


List<Number> numeros = new ArrayList<>();
añadirEnteros(numeros);  // válido

List<Object> objetos = new ArrayList<>();
añadirEnteros(objetos);  // válido


