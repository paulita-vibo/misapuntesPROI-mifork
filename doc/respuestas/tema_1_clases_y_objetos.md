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

Las cuatro características básicas de la Programación Orientada a Objetos (POO) son:

Encapsulamiento
Consiste en ocultar los detalles internos de un objeto y exponer solo lo necesario mediante métodos. Así se protege la información y se evita que otros objetos modifiquen directamente los datos.
-Ejemplo: atributos privados y métodos públicos.

Abstracción
Permite representar solo las características esenciales de un objeto, ignorando los detalles innecesarios. Ayuda a simplificar problemas complejos.
-Ejemplo: una clase Vehículo con métodos como arrancar() sin importar cómo lo hace internamente.

Herencia
Es la capacidad de una clase de heredar atributos y métodos de otra clase, promoviendo la reutilización de código.
-Ejemplo: Auto y Moto heredan de la clase Vehículo.

Polimorfismo
Permite que un mismo método tenga diferentes comportamientos según el objeto que lo implemente.
-Ejemplo: el método sonido() se comporta distinto en un objeto Perro que en un objeto Gato.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Cuatro lenguajes populares que permiten la programación orientada a objetos son:

Java, Python, C++ ,C# 

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada es un paradigma que organiza los programas usando estructuras de control claras y bien definidas, evitando el uso de saltos incontrolados como goto.Se basa principalmente en tres estructuras:
Secuencia: instrucciones que se ejecutan una tras otra.
Selección: decisiones (if, else, switch).
Iteración: bucles (for, while, do-while).


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?
En programación orientada a objetos, un objeto se define por tres elementos fundamentales:
Atributos
Son las propiedades o datos que describen al objeto. Representan su estado.

Métodos
Son las acciones o comportamientos que el objeto puede realizar.

Identidad
Es lo que distingue a un objeto de otro, incluso si tienen los mismos atributos y métodos. Cada objeto es una instancia única.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?
Clase: es un molde o plantilla que define cómo serán los objetos: sus atributos y métodos.
Objeto: es un elemento concreto creado a partir de una clase.
Instancia: es el proceso y el resultado de crear un objeto a partir de una clase.

No son lo mismo:Clase → definición, Objeto → elemento real en memoria
Algunos lenguajes usan prototipos en lugar de clases (por ejemplo, JavaScript).

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En Java, los objetos se almacenan en el heap (montón).

Las referencias a esos objetos suelen estar en la pila (stack).

No es igual en todos los lenguajes:C++ puede usar heap o stack, Java siempre usa heap para objetos.

Recolección de basura es un mecanismo automático que detecta objetos que ya no se usan y libera su memoria automáticamente

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Método: es una función asociada a una clase u objeto, que define su comportamiento.
Sobrecarga de métodos: consiste en definir varios métodos con el mismo nombre, pero con distintos parámetros.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método
    
    class Punto {
        double x;
        double y;
    
        double calculaDistanciaAOrigen() {
            return Math.sqrt(x * x + y * y);
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            Punto p = new Punto();
            p.x = 3;
            p.y = 4;
    
            double d = p.calculaDistanciaAOrigen();
            System.out.println(d); // 5.0
        }
    }


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada es: public static void main(String[] args)

¿Qué es static?-> Significa que el método pertenece a la clase, no a los objetos.Se puede usar sin crear una instancia.

¿Solo se usa en main?
No. Se usa también en métodos, atributos y bloques estáticos.

¿Para qué se combina con final?
static final define constantes:

static final double PI = 3.1416;


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

javac Main.java
java Main
Java se compila, pero no a código máquina.

Se compila a byte-code (.class).

Máquina Virtual de Java (JVM)
Ejecuta el byte-code
Permite que Java sea multiplataforma


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

    class Empleado {
        String dni;
        String nombre;
        String apellidos;
    
        Empleado(String dni, String nombre, String apellidos) {
            this.dni = dni;
            this.nombre = nombre;
            this.apellidos = apellidos;
        }
    }

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

this es una referencia al objeto actual
No se llama igual en todos los lenguajes:
Java, C++ → this
Python → self

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

    double distanciaA(Punto p) {
        return Math.sqrt(
            (this.x - p.x) * (this.x - p.x) +
            (this.y - p.y) * (this.y - p.y)
        );
    }
    

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

En Java todo se pasa por valor

En objetos: se pasa una copia de la referencia
Si modificas atributos del objeto, sí afecta fuera
Si reasignas la referencia, no afecta

En tipos primitivos (int, double):

Cambios no afectan fuera del método

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?
Método que devuelve una representación en texto del objeto

Existe en muchos lenguajes (con otros nombres)


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?
    #include <stdio.h>
    #include <math.h>
    
    typedef struct {
        double x;
        double y;
    } Punto;

    double distanciaAOrigen(Punto *p) {
        return sqrt(p->x * p->x + p->y * p->y);
    }
 ¿Qué pasó con this?
Se convierte en un parámetro explícito (Punto *p).
