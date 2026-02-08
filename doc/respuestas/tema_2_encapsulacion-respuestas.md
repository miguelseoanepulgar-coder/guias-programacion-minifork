<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### La encapsulación en POO busca agrupar dentro de una misma clase tanto los datos como los métodos que operan sobre ellos, formando una unidad coherente,la ocultación consiste en restringir el acceso a esa clase mediane modificadores como "private", esto provoca que el código sea mas claro, mas facil de mantener, y mas consistente.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### La interfaz pública de un objeto o clase en POO se entiende como el conjunto de métodos y atributos accesibles desde fuera de esa clase,esta idea se relaciona directamente con la ocultación de información, ya que solo se hace visible lo que es necesario y util para el resto del programa.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### La interfaz pública de una clase debe diseñarse con cuidado porque constituye el contrato mediante el cual el resto del programa interactúa con ella, además, cambiar la interfaz pública no suele ser fácil. Una vez que otros componentes del programa dependen de ella, modificar nombres de métodos, parámetros o comportamientos puede requerir actualizar numerosas partes del código.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Las invariantes de clase son condiciones o reglas que deben cumplirse siempre para que un objeto se considere en un estado válido. Dichas condiciones deben mantenerse verdaderas tras la construcción del objeto y después de cada operación pública que modifique su estado. La ocultación de información ayuda directamente a preservar estas invariantes porque impide que el código externo pueda modificar los atributos internos libremente.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?



ic class Punto {
    // Estado interno oculto
    private double x;
    private double y;

    // Constructor (parte de la interfaz pública)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Métodos de acceso controlado (parte de la interfaz pública)
    public double getX() { return x; }
    public double getY() { return y; }
}

### Public: accesible desde cualquier parte del código, Private: accesible solo desde la clase.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Los modificadores public y private pueden aplicarse principalmente a clases, atributos y métodos, pero con matices según el tipo de elemento


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### En POO existen más niveles de visibilidad además de la pública y la privada, aunque los disponibles dependen del lenguaje, en Java, existen dos niveles adicionales: "protected" y "package‑private". protected permite el acceso desde la propia clase, desde su paquete y desde sus subclases, incluso si estas están en otro paquete. El acceso package‑private, en cambio, se limita únicamente a las clases del mismo paquete.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### a) para otras clases,a los miembros privados de instancia se puede acceder desde cualquier código de la misma clase, incluso cuando el objeto destino es otra instancia distinta.

public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Accede a campos privados de "otro" porque está dentro de la misma clase
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x; // válido: acceso dentro de la clase Punto
        double dy = this.y - otro.y; // válido: acceso dentro de la clase Punto
        return Math.sqrt(dx * dx + dy * dy);
    }
}


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Los métodos getter y setter son funciones públicas utilizadas en lenguajes orientados a objetos para acceder y modificar de forma controlada los atributos privados de una clase: getter devuelve el valor de un atributo, mientras que un setter permite asignar un nuevo valor.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### No, se refiere a que el los objetos solo se pueden modificar por métodos públicos y no privados y así evitar el riesgo de errores inesperados.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Un miebro de una instancia pertenece a el objeto individual, un miebro de clase es el que pertenece a la clase general por lo que es igual en todos los objetos, y si, si se pueden ocultar.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Sí, tiene sentido que los constructores sean privados en ciertos diseños concretos, aunque no sea lo habitual en la mayoría de las clases. Un constructor privado impide crear objetos desde fuera de la propia clase, lo que puede ser útil cuando se quiere controlar estrictamente cómo y cuándo se generan las instancias.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### En Java, los miembros de clase se indican con la palabra clave static. Un campo o método static pertenece a la clase y no a cada instancia:


public class Punto {
    // --- Estado de instancia (ocultación de información) ---
    private double x;
    private double y;

    // --- Miembros de clase (compartidos por todas las instancias) ---
    // Se mantienen privados y se exponen mediante getters estáticos.
    private static double maxXGlobal = Double.NEGATIVE_INFINITY;
    private static double maxYGlobal = Double.NEGATIVE_INFINITY;

    // Constructor: inicializa el estado y actualiza los máximos globales
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        actualizarMaximosGlobales(x, y);
    }

    // Getters de instancia
    public double getX() { return x; }
    public double getY() { return y; }

    // Setters de instancia (si se permite modificar el punto tras crearlo)
    public void setX(double x) {
        this.x = x;
        actualizarMaximosGlobales(x, this.y);
    }

    public void setY(double y) {
        this.y = y;
        actualizarMaximosGlobales(this.x, y);
    }

    // Comportamiento de instancia
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // --- Interfaz pública de clase (lectura de máximos globales) ---
    public static double getMaxXGlobal() { return maxXGlobal; }
    public static double getMaxYGlobal() { return maxYGlobal; }

    // --- Detalle interno de clase: mantenimiento de máximos ---
    private static void actualizarMaximosGlobales(double x, double y) {
        if (x > maxXGlobal) maxXGlobal = x;
        if (y > maxYGlobal) maxYGlobal = y;
    }
}



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.


public class Punto {
    // Índices para mejorar legibilidad
    private static final int IDX_X = 0;
    private static final int IDX_Y = 1;

    // Representación interna oculta: array de longitud 2
    private final double[] coords = new double[2];

    // Miembros de clase (compartidos)
    private static double maxXGlobal = Double.NEGATIVE_INFINITY;
    private static double maxYGlobal = Double.NEGATIVE_INFINITY;

    // --- Interfaz pública (sin cambios) ---
    public Punto(double x, double y) {
        coords[IDX_X] = x;
        coords[IDX_Y] = y;
        actualizarMaximosGlobales(x, y);
    }

    public double getX() { return coords[IDX_X]; }
    public double getY() { return coords[IDX_Y]; }

    public void setX(double x) {
        coords[IDX_X] = x;
        actualizarMaximosGlobales(x, coords[IDX_Y]);
    }

    public void setY(double y) {
        coords[IDX_Y] = y;
        actualizarMaximosGlobales(coords[IDX_X], y);
    }

    public double calcularDistanciaAOrigen() {
        double x = coords[IDX_X], y = coords[IDX_Y];
        return Math.sqrt(x * x + y * y);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[IDX_X] - otro.coords[IDX_X];
        double dy = this.coords[IDX_Y] - otro.coords[IDX_Y];
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static double getMaxXGlobal() { return maxXGlobal; }
    public static double getMaxYGlobal() { return maxYGlobal; }

    // --- Detalle interno de clase ---
    private static void actualizarMaximosGlobales(double x, double y) {
        if (x > maxXGlobal) maxXGlobal = x;
        if (y > maxYGlobal) maxYGlobal = y;
    }
}



## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### No, no es mejor declarar un atributo público, incluso aunque vaya a tener getter y setter, declarar el atributo como public elimina por completo la capacidad de controlar su uso: cualquier parte del programa podría modificarlo. Obviamente, la convencion más habitual es que sean privados, es el punto principal de encapsulación, que los objetos no puedan ser modificados por métodos externos para mayor control, también entran las invariables de clase, si el códig fuera público, pueden dejar a los objetos en un estado inválido.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Una clase inmutable es aquella cuyo estado no puede cambiar una vez creada una instancia, sus atributos permanecen constantes tras la construcción del objeto y no existen métodos que alteren su contenido interno.Un método modificador es cualquier método que altere el estado interno del objeto, ya sea actualizando atributos, cambiando colecciones internas o modificando estructuras encapsuladas. No es correcto decir que “modificador” es sinónimo de setter: un setter es un caso particular de método modificador, pero hay muchos otros métodos que, sin llamarse setAlgo, también pueden modificar el estado. L a inmutabilidad presenta algunas ventajas, como la simplificaciondel código o que evita errores por invariables.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### No, no es recomendable incluir métodos setter siempre ni como convención general. Su presencia debe justificarse según las necesidades reales del diseño, de nuevo, es el punto de la encapsulacion.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### La clase String en Java es inmutable, lo que significa que su contenido no puede modificarse después de creada. Cada vez que se realiza una operación que “parece” cambiar una cadena como concatenarla, en realidad se está creando un nuevo objeto String con el resultado. Cuando se necesita construir una cadena paso a paso o concatenar muchas veces, lo recomendable es utilizar clases mutables de apoyo, como StringBuilder o StringBuffer.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### En POO, la comparación entre objetos puede hacerse según su identidad o según su contenido, y la elección depende del lenguaje, del diseño y del problema. En Java, el operador == compara identidad, no contenido, de modo que solo devuelve true si ambas referencias apuntan al mismo objeto. El método equals es el mecanismo de Java para comparar objetos por contenido lógico, pero solo si se sobreescribe primero, si no compara identidades, este método se puede usar para comparar strings.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Las clases wrapper son clases que encapsulan un valor de un tipo primitivo dentro de un objeto.La conversión entre un tipo primitivo y su wrapper puede realizarse explícitamente creando un objeto, pero en Java la mayor parte de ese proceso se realiza de manera automática, mediante autoboxing y unboxing. Las clases wrapper ofrecen varias ventajas. Permiten utilizar valores primitivos en APIs que requieren objetos, como las colecciones genéricas (ArrayList<Integer>), y proporcionan métodos útiles para convertir, comparar o formatear esos valores. Además, facilitan trabajar en entornos donde se necesite representar la ausencia de valor (por ejemplo, null), cosa que los tipos primitivos no admiten. También resultan útiles para operaciones genéricas, donde se necesita un tipo parametrizable que represente números o booleanos sin importar su detalle concreto. 


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Un tipo de dato enumerado en POO representa un conjunto finito y cerrado de valores posibles, normalmente asociados a un mismo concepto, en Java, un tipo enumerado es realmente una clase especial, más potente que los enumerados tradicionales de otros lenguajes más simples. Internamente, cada valor del enum es una instancia única y estática de esa clase, lo que permite añadir métodos, atributos y comportamientos propios.Los enumerados en Java ofrecen ventajas claras de encapsulación. En primer lugar, restringen de forma estricta el conjunto de valores permitidos, garantizando que una variable de ese tipo nunca adopte un valor inválido. Además, permiten ocultar detalles internos (atributos privados, constructores privados y métodos auxiliares), exponiendo solo la interfaz pública necesaria. De este modo, el diseño queda más seguro y flexible.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`


public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    // --- Atributos privados (encapsulación) ---
    private final int ordinalAnual; // 1..12
    private final int diasBase;     // 28 en febrero; 30/31 en el resto

    // --- Constructor privado del enum ---
    Mes(int ordinalAnual, int diasBase) {
        this.ordinalAnual = ordinalAnual;
        this.diasBase = diasBase;
    }

    // --- Interfaz pública ---
    /** Devuelve el ordinal del mes en el año: 1..12 */
    public int getOrdinal() {
        return ordinalAnual;
    }

    /** Días "base" del mes (febrero = 28) */
    public int getDias() {
        return (this == FEBRERO) ? 28 : diasBase;
    }

    /** Días del mes considerando si el año es bisiesto (solo afecta a febrero) */
    public int getDias(boolean esBisiesto) {
        return (this == FEBRERO) ? (esBisiesto ? 29 : 28) : diasBase;
    }

    /** Días del mes para un año concreto (regla gregoriana de años bisiestos) */
    public int getDias(int anio) {
        return (this == FEBRERO) ? (esBisiesto(anio) ? 29 : 28) : diasBase;
    }

    // --- Estaciones (convención meteorológica) ---
    public boolean esDePrimavera(boolean esHemisferioNorte) {
        return esHemisferioNorte ? enRango(3, 5) : enRango(9, 11);
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        return esHemisferioNorte ? enRango(6, 8)
                                 : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        return esHemisferioNorte ? enRango(9, 11) : enRango(3, 5);
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        return esHemisferioNorte ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                                 : enRango(6, 8);
    }

    // --- Detalles internos (ocultos) ---
    private static boolean esBisiesto(int anio) {
        return (anio % 400 == 0) || (anio % 4 == 0 && anio % 100 != 0);
    }

    private boolean enRango(int inicioMes, int finMes) {
        return ordinalAnual >= inicioMes && ordinalAnual <= finMes;
    }
}