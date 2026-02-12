<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta: Encapsulamiento: Ocultación o privación de ciertos datos a través de métodos controlados
###            Abstracción: Ignoracia de métodos de implementación y centrarse en lo complejo y esencial para facilitar el cambio
###            Herencia: Reutilizaciíon de clases para crear otras nuevas y establecer jerarquias
###            Polimorfismo: Flexibilidad del método ante diferentes tipos de objeto para poder implementarse de forma diferente


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta: Java, C++, Python y C#.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta: La programacion estructurada es un tipo de programación que se centra en crear su código
###            solo con estructuras de secuencia, selección y repetición(sin goto), y la programación modular se centra
###            en separar el código que realiza funciónes distintas en módulos independientes.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta: estado(valor de los atributos), comportamiento(métodos que todo objeto de la clase puede realizar) e identidad(direccion de memoria).

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta: Una clase es como un plano que define como serán los objetos dentro de ella. Una clase no es lo mismo que un objeto,
###            es la estrucura que los va a definir, creando así una instancia con un estado concreto. No, lenguajes como javascript no usan clases para
###            definir objetos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta: La memoria de objetos se suele almacenar en el "heap"(memoria dinámica), pero en algunos
###            lenguajes como c++, se puede llegar a almacenar en el stack, la ventaja de almacenarla en el heap es que el tamaño se decide en ejecución(dinámico), 
###            y que lo que está en el heap que el método o función donde se a creado, la desventaja es que la info tiene que ser liberada cuando no se necesita,
###            se puede hacer de forma manual o automática   (recolector de basura).
###            La recolleción de basura, consiste en liberar la memoria autamaticamente de los objetos
###            que no tienen uso.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta: Un método es una funcíon dentro de una clase que permite realizar diferentes acciones a los objetos.
###            La sobrecarga de métodos consiste en nombrar igual a dos métodos en una misma clase pero cambiar 
###            los parámetros de cada método y así favorecer el polimorfismo.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta:

class Punto {
    int x;
    int y; 

Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

double calculaDistanciaAOrigen() {
        return Math.sqrt((double) (x * x + y * y));
    }
}

###Ejemplo: 
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto(3, 4);
        double d = p.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + d); // Imprime 5.0
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta: El punto de entrada a un programa de java siempre es el main,La palabra reservada static indica que un miembro pertenece a la clase 
###            y no a una instancia concreta,se puede usar en mas sitios a parte del main, no se necesita oun objeto para usarla y se fuede fusionar con "final" para definir constantes.
###            No se puede usar desde un método static nada que no sea static.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta: Con el comando "javac" se puede complilar el programa en un fichero .class , y con el comando "java" se puede ejecutar. Java si es compilado.
###            La máquina virtual es un entorno de software que simula una computadora idealizada capaz de ejecutar bytecode, que es un lenguaje intermedia al que java 
###            es compilado inicialmente.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta: En java, "new" es un comando que indica que se tiene que reservar memoria para la clase en cuestion. Un contructor es
###            un método especifico posterior a "new" que inicializa el estado de los objetos:


class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor: inicializa el estado del objeto
    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta: La referencia this en Java apunta al objeto actual dentro del contexto de una clase 
###            y se usa para inicializar objetos, pero no se llama igual en todos los lenguajes:


Punto(int x, int y) {
        this.x = x;  // 'this.x' es el campo; 'x' es el parámetro
        this.y = y;
    }



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta:


class Punto {
    int x; 
    int y; 

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt((double) (this.x * this.x + this.y * this.y));
    }

    // Nuevo método: distancia entre 'this' y el punto 'otro'
    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt((double) (dx * dx + dy * dy));
    }
}



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta: En el caso de los objetos, lo que se pasa es la referencia, por tanto los cambios afectaran fuera del método,
###            sin embago, los parámetros se pasan siempre por valor(copia), por tanto su valor externo no cambiará.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta: El método "toString", representa el valor textual del objeto considerado sea del tipo que sea, 
###            también existe en otros lenguajes pero con nombres distintos:


class Punto {
    int x; // visibilidad por defecto (package-private)
    int y; // visibilidad por defecto (package-private)

    Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    double calculaDistanciaAOrigen() {
        return Math.sqrt((double) (x * x + y * y));
    }

    double distanciaA(Punto otro) {
        int dx = this.x - otro.x;
        int dy = this.y - otro.y;
        return Math.sqrt((double) (dx * dx + dy * dy));
    }

    @Override
    public String toString() {
        // Representación clara y compacta del estado del objeto
        return "Punto(" + x + ", " + y + ")";
        // Alternativa: return String.format("Punto(x=%d, y=%d)", x, y);
    }
}

// Uso:
// Punto p = new Punto(3, 4);
// System.out.println(p);        // Imprime: Punto(3, 4)  (equivale a p.toString())



## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta: Aunque una estructura y una clase puedan ser parecidas en el sentido de que agrupan parámetros,
###            le faltarían cosas como los constructores, los modificadores como "static" o "public" y las funciones propias integradas,
###            además, sus variables no estan instanciadas como tal, les faltan cosas como la identidad de objeto y la herencia.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta: Para emularlo, se definiria una estructura con dos parámetros, se definiría una variable punto en el main,
###            y se calcularia de la misma manera que en la función de java. No hay "this" porque la estructura es externa a la clase.
