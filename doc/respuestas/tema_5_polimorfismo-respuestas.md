<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### El polimorfismo es la capacidad de que un mismo mensaje —una llamada a un método— produzca comportamientos distintos según el tipo real del objeto que lo recibe. En programación orientada a objetos se utiliza para permitir que diferentes clases derivadas de un mismo padre puedan responder de manera específica a una misma operación. Su utilidad principal radica en que permite extender programas sin modificar el código existente. Basta con crear nuevas clases que hereden de una clase base y redefinan los métodos necesarios. La sobreescritura de métodos (method overriding) consiste en redefinir en una clase hija un método que ya existe en la clase padre, manteniendo la misma firma. Al hacerlo, la clase hija proporciona su propia versión del comportamiento, que será la que se ejecute cuando el método se invoque sobre un objeto de ese tipo.



## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### La ligadura dinámica o enlace tardío consiste en que la selección del método que debe ejecutarse se decide en tiempo de ejecución y no en tiempo de compilación. Cuando se invoca un método a través de una referencia de tipo padre, el lenguaje determina en ese momento cuál es la implementación concreta que corresponde al tipo real del objeto. Este mecanismo es esencial para que el polimorfismo funcione, ya que permite que distintas clases hijas sobrescriban un mismo método y que la versión adecuada se elija automáticamente durante la ejecución del programa. La relación con el polimorfismo es directa: sin enlace tardío, el polimorfismo no podría existir tal como se entiende en los lenguajes orientados a objetos modernos. El polimorfismo requiere que la llamada a un método se resuelva dinámicamente, de modo que el comportamiento dependa del objeto real y no del tipo de la referencia. Esto permite escribir código genérico que opera sobre tipos base, mientras que las clases derivadas aportan comportamientos especializados sin necesidad de modificar el código que las usa. Respecto a si debe indicarse explícitamente, depende del lenguaje. En Java, el enlace tardío es automático para todos los métodos no static, no final y no private. El programador no necesita marcar nada especial: la JVM se encarga de resolver dinámicamente la llamada.




## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

class Soldado {
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador: Preparado para abrir camino.");
    }
}

class Artillero extends Soldado {
    // No sobrescribe: hereda el saludo del Soldado
}

public class Main {
    public static void main(String[] args) {
        Soldado[] escuadra = new Soldado[3];
        escuadra[0] = new Soldado();
        escuadra[1] = new Zapador();
        escuadra[2] = new Artillero();

        for (Soldado s : escuadra) {
            s.saludar();   // Polimorfismo en acción
        }
    }
}



## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Sí, cuando se sobrescribe un método en Java es posible invocar explícitamente la versión de la clase base y, a partir de ella, añadir comportamiento adicional. Esto permite extender la funcionalidad original sin reemplazarla por completo. Para ello se utiliza la palabra clave super, que sirve para acceder a métodos y atributos heredados tal como estaban definidos en la clase padre.

class Soldado {
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();  // Invoca al método de la clase base
        System.out.println("Zapador: ¡ZAPADOR A SUS ÓRDENES!");
    }
}

class Artillero extends Soldado {
    // Hereda el saludo base sin cambios
}


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Al sobrescribir un método en Java, deben mantenerse exactamente los mismos tipos de parámetros y el mismo nombre. Si se cambia la lista de parámetros, ya no se está sobrescribiendo, sino creando una sobrecarga. En cuanto al tipo de retorno, Java permite que en una sobrescritura sea covariante, es decir, que el método sobrescrito devuelva un tipo más específico que el declarado en la clase base, siempre que sea un subtipo válido. Sin embargo, no se permite cambiarlo por un tipo no relacionado, ya que eso rompería la compatibilidad polimórfica. La diferencia entre sobreescritura (overriding) y sobrecarga (overloading) es fundamental. La sobreescritura ocurre entre clases relacionadas por herencia y redefine un método existente para cambiar su comportamiento en la subclase. La sobrecarga, en cambio, ocurre dentro de la misma clase y consiste en definir varios métodos con el mismo nombre pero con distintas listas de parámetros. La sobreescritura se resuelve en tiempo de ejecución mediante enlace dinámico, mientras que la sobrecarga se resuelve en tiempo de compilación. La anotación @Override sirve para indicar explícitamente que un método pretende sobrescribir otro heredado. Su utilidad principal es que el compilador puede verificar que la sobrescritura es correcta: si el método padre no existe, o la firma no coincide, el compilador generará un error. Esto evita fallos sutiles, como escribir mal el nombre del método o equivocarse en los parámetros, que de otro modo crearían una sobrecarga accidental en lugar de una sobrescritura.



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Sí, cuando se estudia Java se empieza a usar polimorfismo desde el primer momento, incluso aunque no se mencione explícitamente. Cada vez que una clase sobrescribe un método heredado —como toString(), equals() o hashCode()— ya se está participando en el mecanismo polimórfico del lenguaje. Esto ocurre porque esas llamadas se resuelven mediante ligadura dinámica, seleccionando en tiempo de ejecución la versión adecuada del método según el tipo real del objeto. Al sobrescribir toString(), por ejemplo, se está redefiniendo un método que existe en Object, la superclase de todas las clases en Java. Cuando se invoca obj.toString(), la JVM determina en tiempo de ejecución si debe ejecutar la versión de Object o la versión sobrescrita en la clase concreta. Ese comportamiento es exactamente polimorfismo: una misma llamada produce resultados distintos según el tipo real del objeto. Lo mismo ocurre con equals(). Aunque se use para comparar objetos, su funcionamiento también depende del polimorfismo. Si una clase sobrescribe equals(), cualquier código que reciba referencias del tipo Object y llame a equals() ejecutará la versión específica de la clase concreta. Esto permite que cada tipo defina su propia noción de igualdad sin que el código cliente tenga que conocer los detalles.



## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Una clase abstracta es una clase que no puede instanciarse directamente y que está pensada para servir como base de otras clases. Se utiliza cuando se desea definir un comportamiento común para un conjunto de tipos, pero dejando algunos métodos sin implementar porque cada subclase debe proporcionar su propia versión. Una clase abstracta puede contener métodos normales (con cuerpo) y métodos abstractos (sin cuerpo), además de atributos y constructores. Un método abstracto es un método declarado sin implementación, es decir, sin cuerpo. Solo especifica la firma del método, dejando que cada subclase proporcione su propia versión mediante sobrescritura. Esto obliga a que cualquier clase no abstracta que herede de la clase base implemente todos los métodos abstractos. Debido a esta falta de implementación, no es posible crear instancias de una clase abstracta, ya que tendría métodos incompletos que no podrían ejecutarse. 

abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado: ¡A sus órdenes!");
    }

    public abstract void atacar();  // Método abstracto
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Zapador: Colocando explosivos.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Artillero: Preparando el cañón.");
    }
}



## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### La palabra clave final tiene efectos distintos según se aplique a métodos o a clases, y ambos están directamente relacionados con los límites del polimorfismo. Cuando se marca un método como final, se está indicando que no puede ser sobrescrito en ninguna subclase. Esto significa que el comportamiento definido en la clase base queda fijado y no puede modificarse mediante herencia. En consecuencia, se está desactivando la posibilidad de polimorfismo para ese método concreto, ya que todas las llamadas ejecutarán siempre la misma implementación. Cuando se marca una clase como final, se impide que pueda ser heredada. Esto elimina por completo la posibilidad de crear subclases y, por tanto, de aplicar polimorfismo basado en herencia. Una clase final es un tipo cerrado: su comportamiento no puede ampliarse mediante especialización. Esta decisión suele tomarse por motivos de seguridad, eficiencia o diseño, cuando se desea garantizar que el comportamiento de la clase no pueda alterarse. La relación con el polimorfismo es clara: tanto un método final como una clase final limitan o bloquean la capacidad de redefinir comportamientos en subclases. En el caso de los métodos, se impide la sobrescritura; en el caso de las clases, se impide la herencia. Esto contrasta con el uso habitual del polimorfismo, que se basa precisamente en permitir que las subclases redefinan métodos para adaptar o extender el comportamiento. Un ejemplo muy conocido de clase final en la API estándar de Java es java.lang.String. Esta clase es final, lo que impide crear subclases que modifiquen su comportamiento interno. También son final clases como Integer, Double y otros wrappers de tipos primitivos. Esta decisión garantiza que su comportamiento sea seguro, consistente y no pueda alterarse de forma inesperada mediante herencia.



## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Una interfaz en Java es un tipo de referencia que define un conjunto de métodos que una clase se compromete a implementar. A diferencia de una clase normal, una interfaz no describe cómo se hace algo, sino qué operaciones deben estar disponibles. Las interfaces se parecen a las clases abstractas en el sentido de que ambas pueden contener métodos sin implementar y no pueden instanciarse directamente. Sin embargo, una clase abstracta puede tener atributos, constructores y métodos con implementación completa, mientras que una interfaz define esencialmente capacidades o comportamientos que una clase puede adoptar. Además, una clase solo puede heredar de una clase abstracta, pero puede implementar múltiples interfaces, lo que permite una forma de “herencia múltiple” de comportamiento sin los problemas asociados a la herencia múltiple clásica. Una clase puede implementar más de una interfaz, y este es uno de los motivos por los que las interfaces son tan importantes en Java.




## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}


class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Se esperaba un Punto2D");
        }

        Punto2D p = (Punto2D) otro;  // Downcasting seguro
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        return Math.sqrt(dx*dx + dy*dy);
    }
}


class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Se esperaba un Punto3D");
        }

        Punto3D p = (Punto3D) otro;  // Downcasting seguro
        double dx = this.x - p.x;
        double dy = this.y - p.y;
        double dz = this.z - p.z;
        return Math.sqrt(dx*dx + dy*dy + dz*dz);
    }
}


class Linea {
    private Punto a;
    private Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);  // Polimorfismo puro
    }
}



## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### La herencia de interfaces en Java consiste en que una interfaz puede extender a otra, del mismo modo que una clase puede extender a otra clase. Cuando una interfaz extiende a otra, hereda todos sus métodos abstractos (y también sus métodos default o static, si los hubiera). Esto permite construir jerarquías de interfaces que representan capacidades cada vez más específicas, sin necesidad de implementar nada todavía. Es un mecanismo puramente declarativo: solo define contratos que las clases deberán cumplir. En Java sí existe herencia múltiple de interfaces. Una interfaz puede extender a varias interfaces a la vez, y una clase puede implementar tantas interfaces como necesite. Esto es posible porque las interfaces no contienen estado mutable ni lógica compleja que pueda generar conflictos como ocurre con la herencia múltiple de clases. Gracias a esto, Java permite combinar comportamientos de forma flexible y segura, proporcionando una forma controlada de “herencia múltiple”.

interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String texto);
    void eliminar();
}


