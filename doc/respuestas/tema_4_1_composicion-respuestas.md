<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### 
#include <stdio.h>
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto a;  // extremo 1
    Punto b;  // extremo 2
} Linea;

/* Distancia euclidiana entre dos puntos */
double distancia(Punto p1, Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx*dx + dy*dy);
    /* Alternativa numéricamente robusta:
       return hypot(dx, dy);
    */
}

/* Longitud de una línea (distancia entre sus extremos) */
double longitud_linea(Linea l) {
    return distancia(l.a, l.b);
}

int main(void) {
    Punto p1 = {0.0, 0.0};
    Punto p2 = {3.0, 4.0};
    Linea l = {p1, p2};

    printf("Distancia(p1, p2) = %.2f\n", distancia(p1, p2));
    printf("Longitud de la linea = %.2f\n", longitud_linea(l));

    return 0;
}



## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### 
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double x() { return x; }
    public double y() { return y; }

    /** Distancia euclidiana a otro punto */
    public double distanciaA(Punto otro) {
        if (otro == null) {
            throw new IllegalArgumentException("El punto destino no puede ser null");
        }
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.hypot(dx, dy); // numéricamente estable
    }

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}



public final class Linea {
    private final Punto a; // extremo 1
    private final Punto b; // extremo 2

    public Linea(Punto a, Punto b) {
        if (a == null || b == null) {
            throw new IllegalArgumentException("Los extremos no pueden ser null");
        }
        this.a = a;
        this.b = b;
    }

    public Punto extremoA() { return a; }
    public Punto extremoB() { return b; }

    /** Longitud de la línea (distancia entre sus extremos) */
    public double longitud() {
        return a.distanciaA(b);
    }

    @Override
    public String toString() {
        return "Linea{" + a + " -> " + b + "}";
    }
}



## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### La multiplicidad en composición describe cuántas instancias de una clase están vinculadas como parte esencial de otra lo que ayuda a entender la estructura del objeto compuesto. La multiplicidad desde linea a punto es 2, ya que se necesian 2 ptos para formar una linea, y al reves es 0..x, porque no es necesaria una linea para formar un pto, pero un pto puede estar en varias lineas.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### La composición fuerte describe una relación en la que un objeto es propietario exclusivo de los objetos que contiene. En este caso, las “partes” no pueden existir de forma independiente del “todo”: su ciclo de vida está completamente ligado al del objeto que las agrupa. Cuando el objeto principal deja de existir, sus componentes se destruyen con él, ya que carecen de sentido fuera de ese contexto al contrario que en la composición debil, donde los objetos pueden existir libremente.En terminología habitual, lo que se denomina “asociación” o “agregación” corresponde a la composición débil, donde los objetos asociados mantienen identidades y ciclos de vida independientes. Por el contrario, la composición fuerte es la que se conoce habitualmente como “composición” propiamente dicha en UML y en diseño orientado a objetos.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Cuando una clase utiliza a otra solo de manera puntual —por ejemplo, al recibirla como parámetro, devolverla en un método, crearla dentro de un método con new, o manejarla como variable local— no se está ante un caso de composición, sino ante una relación de dependencia. Esta relación indica que una clase necesita a otra temporalmente para realizar una operación concreta, pero no forma parte de su estructura interna ni de su identidad como objeto.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### 
public final class LineaFuerte {
    // Propiedad exclusiva: estas referencias NO se filtran
    private final Punto a;
    private final Punto b;

    /** Se fuerza la creación interna: la línea "posee" estos puntos */
    public static LineaFuerte deCoordenadas(double x1, double y1, double x2, double y2) {
        return new LineaFuerte(new Punto(x1, y1), new Punto(x2, y2));
    }

    /** También se admite entrada por objetos, pero con COPIA defensiva */
    public LineaFuerte(Punto a, Punto b) {
        if (a == null || b == null) throw new IllegalArgumentException("Extremos no pueden ser null");
        // Copias para NO compartir identidad con el exterior
        this.a = new Punto(a.x(), a.y());
        this.b = new Punto(b.x(), b.y());
    }

    /** No se devuelven referencias internas: se devuelven COPIAS */
    public Punto extremoA() { return new Punto(a.x(), a.y()); }
    public Punto extremoB() { return new Punto(b.x(), b.y()); }

    public double longitud() { return a.distanciaA(b); }

    @Override
    public String toString() {
        return "LineaFuerte{" + a + " -> " + b + "}";
    }
}


public final class LineaDebil {
    // Referencias compartidas: NO se copian, pueden pertenecer a otras líneas
    private final Punto a;
    private final Punto b;

    public LineaDebil(Punto a, Punto b) {
        if (a == null || b == null) throw new IllegalArgumentException("Extremos no pueden ser null");
        this.a = a; // sin copia: agregación / dependencia
        this.b = b;
    }

    /** Aquí sí se exponen las mismas referencias (compartibles) */
    public Punto extremoA() { return a; }
    public Punto extremoB() { return b; }

    public double longitud() { return a.distanciaA(b); }

    @Override
    public String toString() {
        return "LineaDebil{" + a + " -> " + b + "}";
    }
}



## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### En una composición fuerte, el contenedor es propietario lógico de sus partes: si el contenedor deja de ser accesible, sus componentes también deberían quedar sin acceso por lo que el recolector de basura se encarga de eliminarlos. Java no permite la destrucción manual. No hay delete, ni destructores, ni métodos que se ejecuten automáticamente al abandonar el ámbito. Tampoco existe el concepto de “objetos en la pila” como en C++: todos los objetos creados con new viven en el heap, y su finalización depende del recolector.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### 

public final class Profesor {
    private final String nombre;
    private final String id; // identificador único lógico (por ejemplo, DNI/legajo)

    public Profesor(String id, String nombre) {
        if (id == null || id.isBlank()) {
            throw new IllegalArgumentException("El id del profesor no puede ser nulo ni vacío");
        }
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser nulo ni vacío");
        }
        this.id = id;
        this.nombre = nombre;
    }

    public String id() { return id; }
    public String nombre() { return nombre; }
}


public final class Departamento {
    private static final int CAPACIDAD_MAX = 50;

    // Encapsulación: no se expone el array
    private final Profesor[] profesores = new Profesor[CAPACIDAD_MAX];
    private int tamaño = 0;

    // Invariante: siempre existe un director y pertenece al array
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director inicial no puede ser null");
        }
        // Arranca con el director formando parte del departamento
        this.director = directorInicial;
        profesores[tamaño++] = directorInicial;
    }

    /** Número de profesores actualmente en el departamento */
    public int numeroProfesores() {
        return tamaño;
    }

    /** Obtiene el profesor por posición (0..numeroProfesores()-1) */
    public Profesor profesorEn(int posicion) {
        validarRango(posicion);
        return profesores[posicion];
    }

    /** Devuelve el director vigente (siempre no-null por invariante) */
    public Profesor director() {
        return director;
    }

    /** Añade un profesor al final, respetando capacidad y evitando duplicados por id */
    public void añadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser null");
        }
        if (tamaño >= CAPACIDAD_MAX) {
            throw new IllegalStateException("Capacidad máxima alcanzada (" + CAPACIDAD_MAX + ")");
        }
        if (indiceDe(p) != -1) {
            throw new IllegalArgumentException("El profesor ya pertenece al departamento: " + p.id());
        }
        profesores[tamaño++] = p;
    }

    /**
     * Elimina el profesor en la posición indicada.
     * No se permite eliminar al director vigente; primero debe cambiarse el director.
     * Devuelve el profesor eliminado.
     */
    public Profesor eliminarProfesorEn(int posicion) {
        validarRango(posicion);
        Profesor eliminado = profesores[posicion];
        if (eliminado.equals(director)) {
            throw new IllegalStateException(
                "No se puede eliminar al director vigente. Cámbiese el director antes de eliminar."
            );
        }
        // Compactación: desplazar a la izquierda
        for (int i = posicion; i < tamaño - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[--tamaño] = null; // evitar fugas de referencia
        return eliminado;
    }

    /**
     * Cambia el director a otro profesor que ya pertenezca al departamento.
     * Se mantiene la invariante: el director siempre está en la lista.
     */
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El nuevo director no puede ser null");
        }
        int idx = indiceDe(nuevoDirector);
        if (idx == -1) {
            throw new IllegalArgumentException(
                "El nuevo director debe pertenecer al departamento: " + nuevoDirector.id()
            );
        }
        this.director = nuevoDirector;
    }

    // --------- Utilidades privadas ---------

    private void validarRango(int posicion) {
        if (posicion < 0 || posicion >= tamaño) {
            throw new IndexOutOfBoundsException(
                "Posición inválida: " + posicion + " (tamaño actual: " + tamaño + ")"
            );
        }
    }

    /** Búsqueda por identidad lógica (equals) para no duplicar el mismo profesor */
    private int indiceDe(Profesor p) {
        for (int i = 0; i < tamaño; i++) {
            if (profesores[i].equals(p)) return i;
        }
        return -1;
    }
}



## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### 
public final class Departamento {
    // Encapsulación: no se expone la lista interna para modificación
    private final List<Profesor> profesores = new ArrayList<>();
    // Invariante: siempre existe un director y pertenece a la lista
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("El director inicial no puede ser null");
        this.director = directorInicial;
        this.profesores.add(directorInicial); // el director siempre pertenece al departamento
    }

    /** Número de profesores del departamento */
    public int numeroProfesores() { return profesores.size(); }

    /** Obtiene el profesor por posición */
    public Profesor getProfesor(int pos) {
        // List#get ya lanza IndexOutOfBoundsException si pos no es válido
        return profesores.get(pos);
    }

    /** Devuelve una vista de solo lectura de los profesores (no modificable desde fuera) */
    public List<Profesor> profesores() {
        return Collections.unmodifiableList(profesores);
        // Alternativa (copia defensiva):
        // return new ArrayList<>(profesores);
    }

    /** Añade un profesor al final (sin duplicados por id) */
    public void añadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("El profesor no puede ser null");
        if (profesores.contains(p)) {
            throw new IllegalArgumentException("El profesor ya pertenece al departamento: " + p.id());
        }
        profesores.add(p);
    }

    /**
     * Elimina al profesor en 'pos'.
     * No se permite eliminar al director vigente; primero debe cambiarse el director.
     * Devuelve el profesor eliminado.
     */
    public Profesor eliminarProfesorEn(int pos) {
        Profesor objetivo = getProfesor(pos); // valida rango
        if (objetivo.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director vigente; cámbiese antes el director.");
        }
        return profesores.remove(pos);
    }

    /** Director actual (no-null por invariante) */
    public Profesor director() { return director; }

    /**
     * Cambia el director por otro profesor que ya pertenezca al departamento.
     * Mantiene la invariante: el director siempre está en la lista.
     */
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("El nuevo director no puede ser null");
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento: " + nuevoDirector.id());
        }
        this.director = nuevoDirector;
    }
}



    public static void main(String[] args) {
        Profesor p1 = new Profesor("ID001", "Ana");
        Profesor p2 = new Profesor("ID002", "Bruno");
        Profesor p3 = new Profesor("ID003", "Carmen");

        Departamento d = new Departamento(p1);   // director inicial y miembro
        d.añadirProfesor(p2);
        d.añadirProfesor(p3);

        System.out.println("Nº profesores: " + d.numeroProfesores());
        System.out.println("Director: " + d.director());

        d.cambiarDirector(p3);
        System.out.println("Nuevo director: " + d.director());

        // Intento de eliminar al director -> excepción esperada
        try {
            int posDir = d.profesores().indexOf(d.director()); // solo lectura
            d.eliminarProfesorEn(posDir);
        } catch (Exception e) {
            System.out.println("Esperada: " + e.getMessage());
        }

        // Eliminación válida
        int posBruno = d.profesores().indexOf(p2);
        System.out.println("Eliminado: " + d.eliminarProfesorEn(posBruno));
        System.out.println("Nº profesores: " + d.numeroProfesores());
    }




## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### 
public final class Persona {
    private final String nombre;
    private final Optional<Persona> madre; // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser nulo ni vacío");
        }
        if (madre != null && madre == this) {
            throw new IllegalArgumentException("Una persona no puede ser su propia madre");
        }
        this.nombre = nombre;
        this.madre = Optional.ofNullable(madre);
    }

    public String nombre() {
        return nombre;
    }

    public Optional<Persona> madre() {
        // Se devuelve tal cual (inmutable por diseño; no hay setters).
        // Si se temiera compartir grafos mutables, se haría copia defensiva.
        return madre;
    }

    @Override
    public String toString() {
        return madre.map(m -> "Persona{nombre='" + nombre + "', madre='" + m.nombre + "'}")
                    .orElse("Persona{nombre='" + nombre + "', madre=<desconocida>}");
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Persona)) return false;
        Persona persona = (Persona) o;
        // Identidad lógica aquí por nombre; en sistemas reales usar un id único
        return nombre.equals(persona.nombre) && madre.equals(persona.madre);
    }

    @Override
    public int hashCode() {
        return Objects.hash(nombre, madre);
    }
}


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Las relaciones de composición bidireccionales son aquellas en las que cada extremo mantiene una referencia al otro y ambos lados se mantienen coherentes: el “todo” conoce a sus “partes” y cada “parte” conoce a su “todo”. En términos de diseño, esto implica gestionar invariantes y operaciones atómicas (o al menos consistentes) para que no aparezcan estados intermedios inválidos. En el ejemplo de Departamento–Profesor, la versión bidireccional significaría que el Departamento mantiene su colección de profesores y cada Profesor conoce el Departamento al que pertenece (normalmente uno; si se necesitase pertenencia múltiple, habría que modelar un conjunto en Profesor).Para implementarlo correctamente, se requiere: (1) métodos que actualicen ambos lados al añadir/eliminar (alta/baja) un profesor; (2) ocultar los mutadores de la relación en Profesor para que únicamente Departamento pueda establecer/desvincular el departamento; (3) mantener la invariante de que el director siempre existe y pertenece a la lista del departamento (no se permite eliminar al director vigente sin cambiarlo antes); y (4) evitar romper la encapsulación al exponer la colección (devolver vista inmodificable o copia).
