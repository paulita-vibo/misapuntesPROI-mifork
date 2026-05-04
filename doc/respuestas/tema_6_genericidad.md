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
EJEMPLO DE VOID EN C 
```java 

#include <stdio.h>
#include <stdlib.h>

typedef struct {
    void** data;
    int size;
    int capacity;
} ArrayGenerico;

ArrayGenerico* crear(int capacity) {
    ArrayGenerico* arr = malloc(sizeof(ArrayGenerico));
    arr->data = malloc(sizeof(void*) * capacity);
    arr->size = 0;
    arr->capacity = capacity;
    return arr;
}

void add(ArrayGenerico* arr, void* elemento) {
    if (arr->size < arr->capacity) {
        arr->data[arr->size++] = elemento;
    }
}

void* get(ArrayGenerico* arr, int index) {
    return arr->data[index];
}
```

USO-> 
```java
int main() {
    ArrayGenerico* arr = crear(10);

    int a = 5;
    float b = 3.14;
    char* texto = "hola";

    add(arr, &a);
    add(arr, &b);
    add(arr, texto);

    printf("%d\n", *(int*)get(arr, 0));
    printf("%f\n", *(float*)get(arr, 1));
    printf("%s\n", (char*)get(arr, 2));

    return 0;
}
```

EJEMPLO OBJECT EN JAVA
```java 
public class ArrayGenerico {
    private Object[] data;
    private int size;

    public ArrayGenerico(int capacity) {
        data = new Object[capacity];
        size = 0;
    }

    public void add(Object elemento) {
        data[size++] = elemento;
    }

    public Object get(int index) {
        return data[index];
    }
}
```
USO-> 
``` java 
public class Main {
    public static void main(String[] args) {
        ArrayGenerico arr = new ArrayGenerico(10);

        arr.add(5);           // autoboxing → Integer
        arr.add(3.14);        // Double
        arr.add("hola");      // String

        int a = (int) arr.get(0);
        double b = (double) arr.get(1);
        String s = (String) arr.get(2);

        System.out.println(a);
        System.out.println(b);
        System.out.println(s);
    }
}
```


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite escribir código independiente del tipo de dato, usando parámetros de tipo (como <T> en Java). Así puedes crear estructuras y algoritmos reutilizables con seguridad de tipos en compilación, sin necesidad de hacer castings manuales.

En lugar de -> Object dato;
    Se usa : T dato;
Donde T es un tipo que se especifica al usar la clase : ArrayGenerico<Integer> numeros;
ArrayGenerico<String> textos;

 ¿El ejemplo anterior es programación genérica?
No .

El ejemplo con Object en Java (o void* en C):

-> Permite almacenar cualquier tipo
-> No es programación genérica real
-> No tiene seguridad de tipos
-> Requiere castings manuales

Es más bien un enfoque “genérico a lo bruto”, pero no usa el mecanismo formal de genéricos.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son el corazón de la programación genérica: permiten escribir código que funciona con distintos tipos de datos sin tener que duplicarlo.

En pocas palabras, un parámetro de tipo es como una “variable de tipo”. En lugar de decir “esto es un int o un String”, dices “esto es de tipo T (o cualquier nombre)”, y ese tipo se concretará más adelante cuando se use el código.
-> Sin Genericos
```java 
public int suma(int a, int b) {
    return a + b;
}
```



## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

No exactamente: Java y C++ no hacen lo mismo cuando usas genéricos. De hecho, siguen dos estrategias casi opuestas.

🔹 ¿Qué hace el compilador al instanciar genéricos?

Depende del lenguaje:

 En Java

El compilador NO crea una versión nueva de la clase por cada tipo.
En su lugar, aplica un proceso llamado:

Borrado de tipos: 
En C++:

El compilador SÍ crea una versión nueva de la clase o función por cada tipo.
Esto se llama:

Instanciación de plantillas

🔹 1. Type Erasure en Java

Cuando escribes:
Caja<String> c1 = new Caja<>();
Caja<Integer> c2 = new Caja<>();
El compilador:

Elimina los parámetros de tipo (T)
Sustituye T por Object (o su límite si lo hay)
Inserta casts automáticos donde hacen falta

🔸 Resultado conceptual

Tu clase:
class Caja<T> {
    T contenido;
}

Se transforma aproximadamente en:

class Caja {
    Object contenido;
}
🔸 Consecuencias

Solo existe UNA clase en tiempo de ejecución
No hay duplicación de código
Compatible con versiones antiguas de Java

Pierdes información de tipo en runtime
No puedes hacer cosas como:

if (caja instanceof Caja<String>) // no permitido

🔹 2. Instanciación de plantillas en C++
En C++:

template <typename T>
class Caja {
    T contenido;
};

Si usas:

Caja<int> c1;
Caja<double> c2;

🔸 ¿Qué hace el compilador?

Genera dos clases distintas:

class Caja_int { int contenido; };
class Caja_double { double contenido; };

🔸 Consecuencias

✔ Código optimizado para cada tipo
✔ No hay casts
✔ Tipos completamente conocidos en compilación

Más uso de memoria (más código generado)
Compilación más lenta

🔹 Diferencia clave (la idea importante)
Java → genéricos “falsos” en runtime (borrados)
C++ → genéricos “reales” (especialización por tipo)


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

```java 
class Par<Q, P>{
    private final P primero; 
    private final Q segundo; 

    public Par (P primero, Q segundo){
        this.primero= primero; 
        this.segundo= segundo; 
    }
    public P getSegundo(){
        return this.segundo; 
    }
    public P getPrimero(){
        return this.primero; 
    }

}

class Estadisticas{
    public static Par<Double, Double> mediaYDesviacionTipica(List<Double> valores){
        //...
        double media=....
        double stddev=... 
        
        return new Par<Double, Double>(media, stddev); 
    }

    //uso media y desviacion tipica mediaYDesviacionTipica
    List<Double>valores=....; 
    Par mediaYDesviacionTipica= mediaYDesviacionTipica(valores); 
    double media = (Double) mediaYDesviacionTipica.getPrimero(); 
    Par resultadoAlumno=...
    Alumno alumno= (Alumno) resultadoAlumno.getPrimero(); 
}
/////////////////////////////
class Par<P,Q>{
    private Object primero; 
    private Object segundo; 
    
    public Par(Object primero, Object segundo){
        this.primero= primero; 
        this.segundo= segundo;
    }

    public P getPrimero

}
/////////////////////////////
class Colegio{
    public static List<Par<Alumno, Double>>obtenerAlumnosYsusNotas(){

    }
}
```



## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

🔹 Version SIN genericos (usando Object)
```java 
import java.util.Random;

public class EjemploSinGenericos {

    public static Object seleccionaUno(Object a, Object b) {
        Random rand = new Random();
        return rand.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s = "Hola";
        Integer n = 42;

        // Permite mezclar tipos distintos
        Object resultado = seleccionaUno(s, n);

        // Necesita downcasting
        String texto = (String) resultado; // Puede fallar en tiempo de ejecución
        System.out.println(texto);
    }
}
```
Problemas:
 Necesita downcasting
No garantiza que ambos parámetros sean del mismo tipo
Puede lanzar ClassCastException

🔹 Versión CON método genérico

```java 
import java.util.Random;

public class EjemploConGenericos {

    public static <T> T seleccionaUno(T a, T b) {
        Random rand = new Random();
        return rand.nextBoolean() ? a : b;
    }

    public static void main(String[] args) {
        String s1 = "Hola";
        String s2 = "Adiós";

        // Solo acepta argumentos del mismo tipo
        String resultado = seleccionaUno(s1, s2);

        // No necesita casting
        System.out.println(resultado);

        // Esto NO compila (tipos distintos)
        // seleccionaUno("Hola", 42);
    }
}
```

🔹 Diferencias clave
(i) Evitar downcasting
Con Object → necesitas cast:
String texto = (String) resultado;
Con genéricos → no hace falta:
String resultado = seleccionaUno(s1, s2);

(ii) Forzar mismo tipo
Con Object → permite mezclar tipos:
seleccionaUno("Hola", 42); // ✔ compila (pero es peligroso)
Con <T> → obliga a que sean del mismo tipo:
seleccionaUno("Hola", 42); //  error de compilación



## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?
🔹 Solución 1: usando directamente Number
```java 
public class PuntoNumber {

    private Number x;
    private Number y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static void main(String[] args) {
        PuntoNumber p1 = new PuntoNumber(1, 2);          // Integer
        PuntoNumber p2 = new PuntoNumber(3.5, 4.5);      // Double

        double d = p1.calcularDistanciaA(p2);
        System.out.println(d);
    }
}

```
🔹 Solución 2: usando genéricos con restricción
```java 
public class Punto<T extends Number> {

    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static void main(String[] args) {
        Punto<Integer> p1 = new Punto<>(1, 2);
        Punto<Integer> p2 = new Punto<>(3, 4);

        double d = p1.calcularDistanciaA(p2);
        System.out.println(d);

        // ❌ Esto NO compila (tipos distintos)
        // Punto<Integer> p3 = new Punto<>(1, 2);
        // Punto<Double> p4 = new Punto<>(3.0, 4.0);
        // p3.calcularDistanciaA(p4);
    }
}
```

🔹 Diferencias clave
✔ Con Number
Permite mezclar tipos (Integer, Double, etc.)
Menos seguro
Más flexible pero menos preciso
✔ Con <T extends Number>
Obliga a trabajar con un único tipo concreto
Mayor seguridad en compilación
Mejora la expresividad del código



## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

🔹 ¿Se pueden mezclar tipos en las coordenadas?

-> Solución sin genéricos (Number):

PuntoNumber p = new PuntoNumber(1, 2.5);
Sí lo permite x puede ser Integer y y Double
No hay control: mezcla tipos libremente

-> Solución con genéricos (<T extends Number>):

Punto<Integer> p = new Punto<>(1, 2);      // ✔ válido
Punto<Double> p2 = new Punto<>(1.0, 2.5);  // ✔ válido
Punto<?> p = new Punto<>(1, 2.5); //  no compila con <T>
No permite mezclar tipos dentro del mismo punto
Obliga a que ambas coordenadas sean del mismo tipo T 

🔹 Tipo devuelto por getX
Sin genéricos (Number)
public Number getX()
🔸 Devuelve Number
Pierdes información concreta del tipo
Necesitas casting si quieres algo específico:
Integer x = (Integer) p.getX(); // riesgo en runtime


Con genéricos (<T extends Number>)
public T getX()
🔸 Devuelve el tipo exacto (Integer, Double, etc.)
No necesitas casting
Punto<Integer> p = new Punto<>(1, 2);
Integer x = p.getX(); // ✔ seguro


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


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

No, las dos cosas no funcionan igual, y ahí está justo el punto interesante:

1) List<String> vs List<Object>

Aunque String es subtipo de Object, List<String> NO es subtipo de List<Object>.

¿Por qué?
Porque los genéricos en Java son invariantes. Si se permitiera esa relación, podrías hacer algo peligroso:
```java 
List<String> strings = new ArrayList<>();
List<Object> objects = strings; // ← si esto fuera válido

objects.add(42); // añades un Integer
```
Ahora strings contendría un Integer, rompiendo la seguridad de tipos.
El compilador evita esto prohibiendo esa conversión.

2) String[] vs Object[]

Aquí sí: String[] SÍ es subtipo de Object[].
```java 
String[] strings = new String[2];
Object[] objects = strings; // válido
```
Pero esto introduce un problema en tiempo de ejecución:
```java
objects[0] = 42; // compila, pero...
```
👉 Esto lanza una excepción en runtime: ArrayStoreException.

¿Por qué?
Porque los arrays en Java son covariantes, pero mantienen un control dinámico del tipo real del array. En este caso, el array realmente es de String, así que no permite guardar un Integer.


3) ¿Por qué la diferencia?
Genéricos (List<T>): comprobación en tiempo de compilación → más seguros → invariantes.
Arrays (T[]): diseño antiguo de Java → covariantes → comprobación parcial en runtime → menos seguros.


4) Definiciones clave

A partir de estos ejemplos:

✔️ Covariante

Un tipo genérico es covariante si:

Si A es subtipo de B, entonces F<A> es subtipo de F<B>.

Ejemplo:

Arrays en Java: String[] → Object[]

Problema: puede romper la seguridad de tipos (como vimos).

✔️ Contravariante

Un tipo es contravariante si:

Si A es subtipo de B, entonces F<B> es subtipo de F<A>.

Ejemplo típico en Java:

List<? super String>

Permite meter String, pero no garantiza qué tipo exacto se obtiene al leer.

✔️ Invariante

Un tipo es invariante si:

Aunque A sea subtipo de B, F<A> y F<B> no tienen relación de subtipado.

Ejemplo:

List<String> y List<Object> → no son compatibles

***CLASE 
String [] miarrayS= {"A", "B"}
Object [] mirray0=miarrayS; 

en el heap tengo el String [] y en el Stack miarray 
-> Sí, es tipo covariante 

list<String> no es tipo combatible con List<Object>
List<String> miListas= ...
List<Object> miLista0 = miListas; -> Compliador los genericos son invariantes


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

🔹 ¿Qué es un wildcard (?)?

Un wildcard (?) representa un tipo desconocido.

Permite introducir covarianza y contravarianza controladas en genéricos.

🔹 ? extends T vs ? super T

? extends T (covariante)
Significa: “algún subtipo de T”
✔Se usa para leer
No puedes añadir elementos (excepto null)
List<? extends Number>

? super T (contravariante)
Significa: “algún supertipo de T”
Se usa para escribir
Al leer, solo obtienes Object
List<? super Integer>

🔹 Ejemplo (i): sumar números (? extends)
```java
import java.util.List;

public class EjemploExtends {

    public static double suma(List<? extends Number> lista) {
        double total = 0;
        for (Number n : lista) {
            total += n.doubleValue();
        }
        return total;
    }
}
suma(List.of(1, 2, 3));        // Integer
suma(List.of(1.5, 2.5, 3.5));  // Double
```

🔹 Ejemplo (ii): añadir enteros (? super)
```java 
import java.util.List;

public class EjemploSuper {

    public static void agregarEnteros(List<? super Integer> lista) {
        lista.add(1);
        lista.add(2);
        lista.add(3);
    }
}import java.util.List;

public class EjemploSuper {

    public static void agregarEnteros(List<? super Integer> lista) {
        lista.add(1);
        lista.add(2);
        lista.add(3);
    }
}

List<Number> lista1 = new ArrayList<>();
List<Object> lista2 = new ArrayList<>();

agregarEnteros(lista1);
agregarEnteros(lista2);
```

***CLASE 
<?>
<? super Number>
<? Extends Number>

List <String> miLista 
List <?> miLista 
List <? super Number>
List<? extends Number>
