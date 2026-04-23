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

### Respuesta


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta


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

### Respuesta


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
