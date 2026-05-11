<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### En orientación a objetos, la herencia es un mecanismo que permite definir una clase nueva a partir de otra ya existente, reutilizando sus características,  Esta relación suele describirse como “A es-un B”: si una clase Artillero hereda de Soldado, se afirma que un artillero es un soldado. No se trata solo de similitud conceptual, sino de una relación formal dentro del sistema de tipos del lenguaje. La herencia permite organizar las clases de forma jerárquica y modelar especializaciones de un concepto más general.La primera implicación importante de la herencia es la compatibilidad de tipos. Cuando una clase hereda de otra, los objetos de la subclase pueden ser tratados como objetos de la superclase. Esto significa que cualquier lugar donde se espere un Soldado puede usarse un Artillero o un Zapador sin causar errores de tipo.La segunda implicación es la herencia de estado y comportamiento. Las subclases heredan los atributos y métodos no privados de la superclase, evitando duplicación de código. En el ejemplo, tanto Artillero como Zapador heredan el atributo nombre y el método saludar(), que define un comportamiento común a todos los soldados. Cada subtipo añade después su propio estado específico (cohetes o minas) y sus métodos asociados, combinando así reutilización con especialización.


class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 10);
        ejercito[2] = new Artillero("Ana", 3);

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Al crear un objeto de una clase concreta que hereda de otra (por ejemplo, Artillero o Zapador), siempre se ejecutan dos constructores: el de la clase base (Soldado) y el de la clase concreta. El orden de ejecución es fijo: primero se ejecuta el constructor de la superclase y, a continuación, el de la subclase. La palabra clave super dentro de un constructor se utiliza para invocar un constructor de la clase base. Si la clase base no tiene visible (public o protected) un constructor sin parámetros, entonces es obligatorio llamar explícitamente a super con los argumentos adecuados. De lo contrario, el código no compila, ya que Java no puede inventar una forma de construir correctamente la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### En una jerarquía de herencia, los atributos privados de la superclase sí forman parte de la instancia en memoria de la subclase. Cuando se crea un objeto de una subclase como Artillero, la instancia resultante contiene internamente la parte correspondiente a Soldado (con todos sus atributos, incluidos los privados) y, además, la parte específica de Artillero. Sin embargo, que esos atributos existan físicamente en memoria no implica que puedan usarse directamente desde el código de la subclase. En Java, el modificador private restringe el acceso al código de la propia clase donde se declara el atributo, independientemente de la herencia. Por tanto, aunque un Artillero tenga internamente el atributo nombre heredado de Soldado, ese atributo no puede ser referenciado directamente desde la clase Artillero. Esto se observa claramente en el ejemplo de Soldado. El atributo nombre es privado, por lo que solo puede ser usado dentro de la clase Soldado. La subclase Artillero no puede escribir nombre ni leer su valor directamente. La forma correcta de interactuar con ese estado heredado es a través de métodos de la superclase, como saludar() o, si existiera, un método getNombre().

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### La compatibilidad de tipos que introduce la herencia tiene una implicación directa en la extensibilidad del código. Al poder tratar todos los objetos de las subclases como si fueran instancias de la superclase (Soldado), el código que trabaja con el tipo base queda abierto a nuevas extensiones sin necesidad de ser modificado. Esto significa que puede añadirse un nuevo tipo de soldado y el código existente seguirá funcionando correctamente, siempre que ese nuevo tipo respete el contrato definido por la superclase. Desde el punto de vista del diseño, esto reduce el acoplamiento entre las partes del sistema. El código que “usa” soldados no necesita conocer los detalles concretos de cada subtipo, solo necesita saber que son Soldado y que, por tanto, pueden ejecutar operaciones comunes como saludar(). Esta idea conecta con el principio de abierto/cerrado: el sistema puede ampliarse añadiendo nuevas clases, pero no es necesario reescribir el código ya existente para soportarlas.


class Francotirador extends Soldado {
    private int balasPrecision;

    public Francotirador(String nombre, int balasPrecision) {
        super(nombre);
        this.balasPrecision = balasPrecision;
    }

    public int getBalasPrecision() {
        return balasPrecision;
    }
}



Soldado[] ejercito = new Soldado[4];
ejercito[0] = new Artillero("Carlos", 5);
ejercito[1] = new Zapador("Luis", 10);
ejercito[2] = new Artillero("Ana", 3);
ejercito[3] = new Francotirador("Marta", 20);

for (Soldado s : ejercito) {
    s.saludar();
}



## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### En Java es perfectamente válido que una referencia del supertipo apunte a un objeto real de un subtipo. Por ejemplo, una variable de tipo Soldado puede referenciar un objeto Artillero o Zapador. Esto es una consecuencia directa de la herencia y de la compatibilidad de tipos: todo objeto de una subclase es también un objeto de la superclase. Sin embargo, el tipo de la referencia determina qué métodos pueden invocarse en tiempo de compilación. Con una referencia del supertipo solo pueden invocarse los métodos declarados en el supertipo, incluso aunque el objeto real sea de una subclase. No es posible llamar directamente a métodos públicos específicos del subtipo (como getCohetes() de Artillero) usando una referencia de tipo Soldado, porque el compilador no puede garantizar que ese método exista para cualquier Soldado. No obstante, si el método está sobrescrito (override), se ejecutará en tiempo de ejecución la versión correspondiente al subtipo, lo que constituye el polimorfismo dinámico.El upcasting consiste en tratar un objeto de un subtipo como si fuera del supertipo, y se realiza de forma automática y segura. Ocurre, por ejemplo, al asignar un Artillero a una referencia Soldado. El downcasting, por el contrario, consiste en convertir una referencia del supertipo al subtipo real del objeto. Este proceso requiere una conversión explícita y puede provocar un error en tiempo de ejecución si el objeto no es realmente de ese subtipo. Para evitar errores al hacer downcasting, Java proporciona el operador instanceof, que permite comprobar el tipo real del objeto en tiempo de ejecución. En el siguiente ejemplo se recorre un array de Soldado y, solo si el objeto real es un Artillero, se accede de forma segura a su número de cohetes:


for (Soldado s : ejercito) {
    s.saludar();

    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // downcasting seguro
        System.out.println("Cohetes disponibles: " + a.getCohetes());
    }
}



## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### En el contexto de la ocultación de información y la herencia, el acceso protegido permite un equilibrio entre encapsulación y reutilización. Un atributo o método protegido no es accesible desde cualquier clase, pero sí puede ser utilizado por las subclases, además de por las clases del mismo paquete. Esto resulta útil cuando se desea que ciertos detalles internos no sean completamente públicos, pero sí estén disponibles para las clases que especializan el comportamiento de una superclase. En Java, el acceso protegido se implementa mediante la palabra clave protected. A diferencia de private, que restringe el acceso exclusivamente a la propia clase, protected permite que las subclases accedan directamente al atributo o método heredado. De este modo, se mantiene un cierto control sobre el estado interno, sin impedir que las subclases lo utilicen para implementar comportamientos más específicos. En el ejemplo de Soldado, hacer que el atributo nombre sea protegido permite que una subclase como Zapador lo use para personalizar su comportamiento, por ejemplo al mostrar un mensaje al colocar una bomba. El atributo sigue sin ser accesible desde código externo no relacionado, pero deja de estar completamente oculto para las clases hijas, lo que facilita la extensión controlada del comportamiento.


class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerBomba() {
        System.out.println("El zapador " + nombre + " ha colocado una mina");
    }

    public int getMinas() {
        return minas;
    }
}



## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### En los lenguajes orientados a objetos no existe una regla universal que obligue a que todos los objetos deriven de una única clase base común. La existencia de una “clase raíz” depende del diseño de cada lenguaje. Algunos lenguajes optan por jerarquías explícitas y unificadas, mientras que otros permiten modelos más flexibles o incluso mixtos, donde no todos los tipos comparten un ancestro común visible para el programador. En Java, sí existe una clase base común para todos los objetos: la clase Object, perteneciente al paquete java.lang. Toda clase creada en Java hereda directa o indirectamente de Object, incluso aunque no se indique explícitamente con extends. Esto significa que cualquier objeto en Java dispone, como mínimo, de los métodos definidos en Object, como toString(), equals() o getClass(), lo que facilita el manejo uniforme de objetos en el lenguaje.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### La herencia múltiple es un mecanismo de la orientación a objetos por el cual una clase puede heredar directamente de más de una clase base. Esto implica que la clase derivada adquiere el estado y el comportamiento de todas sus superclases.No esta soportada en java.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### En los lenguajes orientados a objetos, las excepciones son objetos, lo que significa que se definen mediante clases y se organizan en una jerarquía de herencia. Esto permite crear excepciones personalizadas que representen errores propios del dominio del problema, en lugar de reutilizar excepciones genéricas. En Java, una excepción será no controlada cuando herede de RuntimeException, lo que implica que no es obligatorio capturarla ni declararla con throws. Al ser objetos, las excepciones también pueden componerse con otros objetos, de la misma forma que cualquier otra clase. En este caso, la excepción UsuarioNoEncontradoException puede contener un objeto Usuario para indicar qué usuario concreto provocó el error. Esto resulta especialmente útil para depuración y para manejar errores de forma más rica, ya que la excepción no solo informa de que algo falló, sino también del contexto del fallo. Java permite además encadenar excepciones, es decir, asociar una causa subyacente a una excepción. Esto se logra sobrecargando los constructores y delegando en los constructores de la superclase (RuntimeException). De este modo, la excepción personalizada puede incluir tanto información propia del dominio (el Usuario) como la excepción original que causó el problema, sin perder trazabilidad.


class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}



## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### En orientación a objetos, no se recomienda usar herencia únicamente como mecanismo de reutilización de código porque la herencia introduce una relación semántica fuerte entre clases, no solo técnica. Cuando se utiliza herencia, se afirma explícitamente que “A es-un B”, es decir, que la subclase representa una especialización válida de la superclase en todos los contextos. Si esa relación no es cierta desde el punto de vista del modelo del problema, la herencia genera jerarquías artificiales y frágiles, aunque inicialmente permita ahorrar código. Además, la herencia crea un alto acoplamiento entre la subclase y la superclase. La subclase depende directamente de la implementación de la clase base, no solo de su interfaz pública. Cambios en la superclase (nuevos métodos, modificaciones de comportamiento, cambios en atributos protegidos) pueden afectar de forma inesperada a todas las subclases, incluso a aquellas que solo heredaban para “reutilizar código”. Esto dificulta el mantenimiento y la evolución del sistema a medio y largo plazo. La composición, en cambio, permite reutilizar código sin establecer una relación de tipo jerárquico. Cuando una clase tiene-un objeto de otra clase (en lugar de ser-un tipo de ella), se mantiene la encapsulación y se reduce el acoplamiento. La clase compuesta depende del comportamiento expuesto por la otra clase, no de su estructura interna, lo que hace el diseño más flexible y menos sensible a cambios. Por este motivo se suele decir: “favorecer la composición frente a la herencia”.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Se recomienda favorecer la composición frente a la herencia porque la composición genera diseños más flexibles, desacoplados y fáciles de mantener. Mientras que la herencia establece una relación rígida y permanente (A es‑un B), la composición establece una relación más débil y modular (A tiene‑un B). Esto permite construir comportamientos combinando objetos, sin imponer una jerarquía fija que condicione todo el diseño del sistema. La herencia introduce un acoplamiento fuerte entre la clase base y las subclases. Una subclase no solo hereda una interfaz, sino también una implementación concreta, lo que hace que cambios en la clase base puedan afectar de forma indirecta y difícil de prever a todas las subclases. Esto provoca diseños frágiles, especialmente cuando la herencia se utiliza solo para reutilizar código y no porque exista una verdadera relación de especialización conceptual. La composición, en cambio, permite variar el comportamiento en tiempo de ejecución y sustituir componentes sin modificar la jerarquía de clases. Una clase delega parte de su comportamiento a otros objetos, usando sus interfaces públicas, lo que refuerza la encapsulación y reduce dependencias internas. Este enfoque encaja mejor con sistemas extensibles, donde se espera que el software evolucione con nuevos requisitos.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Cuando se afirma que la herencia rompe la encapsulación, se hace referencia a que las subclases quedan expuestas a detalles internos de la superclase, rompiendo la idea de que una clase debería ocultar su implementación y mostrar solo una interfaz bien definida.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.


// Modelo con herencia
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}



// Modelo con composición
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        thisdatos = datos;
    }
}

