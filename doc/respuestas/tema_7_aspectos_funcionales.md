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

Un puntero a función en C es una variable que almacena la dirección de memoria de una función. Permite pasar funciones como argumentos, almacenarlas en estructuras o invocarlas indirectamente.

Ejemplo solicitado

Definimos una función que recibe una cadena (char *) y la convierte a mayúsculas (modificando la cadena original), y luego usamos un puntero a esa función:

```java 
#include <stdio.h>
#include <ctype.h>

// Función que convierte una cadena a mayúsculas
char* convertirAMayusculas(char *cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper((unsigned char)cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "Hola mundo";

    // Definición del puntero a función
    char* (*aMayusculas)(char *);

    // Asignación de la función al puntero
    aMayusculas = convertirAMayusculas;

    // Invocación mediante el puntero
    char *resultado = aMayusculas(texto);

    printf("Resultado: %s\n", resultado);

    return 0;
}
```

char* (*aMayusculas)(char *);
Declara un puntero a función que:
recibe char *
devuelve char *
aMayusculas = convertirAMayusculas;
Asigna la dirección de la función.
aMayusculas(texto);
Llama a la función a través del puntero (igual que si llamaras directamente).

***CLASE 
-Las funciones son "ciudadanos de primera clase".Una función es un tipo más: 
    -Se puede asignar a una variable 
    -Se puede pasar como parámetro
    -Se puede una función como retorno de otra 
-Clousure //paradigma funcional
-Expresiones Lambda -> no tienen nombre 
-En lenguajes con coprobación estática de tipos ; ¿Qué tipo tienen?


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima (sin nombre) que se puede definir en línea y tratar como un valor: asignarla a una variable, pasarla como argumento o devolverla desde otra función. Suele usarse para código corto y expresivo.

Ejemplo en JavaScript
```java 
// Función lambda (arrow function)
const aMayusculas = (cadena) => {
  return cadena.toUpperCase();
};

// Invocación
const texto = "Hola mundo";
const resultado = aMayusculas(texto);

console.log(resultado); // HOLA MUNDO
```

En JavaScript:
(cadena) => ... define la lambda
Se asigna a la variable aMayusculas
Se usa como cualquier función

Ejemplo en Java
En Java, las lambdas se usan con interfaces funcionales como Function<T, R>.
```java 
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Función lambda
        Function<String, String> aMayusculas = (cadena) -> {
            return cadena.toUpperCase();
        };

        // Invocación
        String texto = "Hola mundo";
        String resultado = aMayusculas.apply(texto);

        System.out.println(resultado); // HOLA MUNDO
    }
}
```
En Java:

(cadena) -> ... define la lambda
Function<String, String> indica:
entrada: String
salida: String
Se invoca con .apply()

***CLASE 
Function <String, String> mifuncion = s-> s.topUpperCase(); 

en java: 
```java 
String entrada = "hola"; 
String salida = mifuncion.apply (entrada);   //apply solo si es un function
```
en javaSript -> hay que poner (entrada) y ya ns que mas 




## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación en el que el programa se construye principalmente mediante la composición de funciones. Se centra en:

Evitar estados mutables (datos que cambian)
Evitar efectos secundarios (que una función modifique algo fuera de ella)
Usar funciones puras (mismo input → mismo output)
Tratar las funciones como valores que se pueden combinar

En lugar de decir “haz esto paso a paso”, se expresa más como “qué resultado quiero obtener a partir de estas transformaciones”.

¿Por qué Java 8 es multi-paradigma?

Lenguajes como Java, tradicionalmente orientados a objetos, se consideran multi-paradigma porque permiten usar varios estilos de programación en el mismo lenguaje:

✔ Orientado a objetos (clases, objetos, herencia)
✔ Imperativo (instrucciones paso a paso)
✔ Funcional (desde Java 8)

A partir de Java 8 se añadieron características funcionales como:

Funciones lambda
Interfaces funcionales (Function, Predicate, etc.)
Streams (map, filter, reduce)

Esto permite escribir código más declarativo, por ejemplo:
```java 
lista.stream()
     .map(s -> s.toUpperCase())
     .filter(s -> s.length() > 3)
     .forEach(System.out::println);
```
¿Qué significa que las funciones son "ciudadanos de primera clase"?

Que las funciones se pueden usar igual que cualquier otro valor (como un número o un objeto). Es decir, puedes:

✔ Asignarlas a variables
✔ Pasarlas como argumentos
✔ Devolverlas desde otras funciones
✔ Almacenarlas en estructuras (listas, arrays, etc.)

Ejemplo conceptual
En JavaScript:
```java
const saludar = (nombre) => "Hola " + nombre;

function ejecutar(f) {
  return f("Ana");
}

console.log(ejecutar(saludar));
```
Aquí la función saludar:

Se guarda en una variable
Se pasa como argumento
Se ejecuta dinámicamente

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis básica de una función lambda en Java sigue este esquema:


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.




## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

En C no hay Clousure 
Aunque a primera vista parecen similares (ambos permiten “usar funciones como valores”), no son lo mismo ni tienen el mismo poder ni seguridad.

Diferencia fundamental
Puntero a función en C → es solo una dirección de memoria de una función existente.
Lambda (Java, JavaScript, etc.) → es un objeto/valor que puede capturar contexto y comportarse como una función.
Diferencias clave
1) Captura de variables (closures)
 C (punteros a función):
No pueden capturar variables del entorno.
Lambdas:
Pueden “recordar” variables externas (closure).
```java 
int x = 10;
Function<Integer, Integer> f = y -> x + y; // captura x
```
2) Naturaleza del objeto
C:
Un puntero a función es literalmente una dirección:
```java 
int (*f)(int);
```
Java (lambda):
Es una instancia de una interfaz funcional (un objeto con comportamiento).

3) Seguridad de tipos
C:
Más flexible, pero menos seguro (puedes equivocarte con tipos de punteros).
Lambdas (Java):
Tipado fuerte y verificado en compilación.

4) Expresividad
C:
No hay funciones anónimas estándar (hasta C moderno con extensiones)
Código más verboso para callbacks
Lambdas:
Funciones anónimas inline
Código más compacto y declarativo

5) Paradigma
C:
Imperativo/procedimental
Lambdas:
Parte del paradigma funcional (composición, inmutabilidad, etc.)

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

***CLASE 
```java 
static Function <Double, Double> crearDescuento (int porcentaje){
    return cantidad-> cantidad * (1-pocentaje/100.0)
}
```

¿En que momento estamos activando closures? -> Function <Double, Double>
                                                return cantidad-> cantidad * (1-pocentaje/100.0)
                    
```java 
static Function <Double, Double> crear Descuento (int porcentaje){
    return cantidad-> cantidad * (1-pocentaje/100.0)
}
public void main (){
    Function <Double, Double> descuento25 = crearDescuento (25); 
    Function <Double, Double> descuento50 = crearDescuento (50); 
    double descontado = descuento25.apply (4000); 
    descontado = descuento50.apply(500); 
}
```


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?
En Java, una interfaz funcional es una interfaz que define exactamente un único método abstracto, y cuyo propósito es servir como “tipo objetivo” para una función lambda o una referencia a método.

✔️ ¿Qué es exactamente?

Es una interfaz que representa una operación (una función).
Cuando escribes una lambda, en realidad estás creando una instancia de esa interfaz.

Ejemplo:
```java 
Function<String, String> aMayusculas = s -> s.toUpperCase();
```
Aquí:

Function<String, String> es la interfaz funcional
La lambda s -> s.toUpperCase() es su implementación

✔️ Requisitos de una interfaz funcional
1.Un único método abstracto (SAM: Single Abstract Method)
Este es el requisito clave.
```java 
@FunctionalInterface
interface MiFuncion {
    int aplicar(int x); // único método abstracto
}
```
2.Puede tener métodos default o static
Estos no cuentan como abstractos, así que están permitidos:
```java 
@FunctionalInterface
interface MiFuncion {
    int aplicar(int x);

    default void imprimir() {
        System.out.println("Hola");
    }

    static void utilidad() {
        System.out.println("Util");
    }
}
```
3.Puede sobrescribir métodos de Object
Tampoco cuentan como abstractos adicionales:
```java 
boolean equals(Object o); // permitido
```
4.Uso opcional de la anotación @FunctionalInterface
No es obligatoria
Pero es recomendable: el compilador verifica que cumples la regla

✔️ Ejemplos de interfaces funcionales estándar

Java ya incluye muchas en java.util.function:

Function<T, R> → recibe T, devuelve R
Predicate<T> → devuelve boolean
Consumer<T> → no devuelve nada
Supplier<T> → no recibe nada


✔️ Idea clave
Una interfaz funcional es el puente entre el tipado estático de Java y las funciones lambda:

Define “la forma” de la función (firma)
La lambda aporta “el comportamiento”


***CLASE

Es una iterfaz que solo tiene un unico metodo abstracto 
si ademas nosotros queremos asegurarnos y no equivocarnos de no poner solo 1 metodo abstracto -> poner @FunctionalInterface

```java 
@FunctionalInterface
public Interface Transformador {   //no hace falta poner abstract 
    public String transformar (String entrada); 
}
```

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Ahora podriamos hacer 
```java 
@FunctionalInterface
public Interface Transformador {   //no hace falta poner abstract 
    public String transformar (String entrada); 

}
public void main (){
    Transformador miTransformador = S-> s.toUpperCase(); 
    String entrada = "hola"; 
    String salida = miTransformador.transformar (entrada); // ya no hay apply
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

```java 
@FunctionalInterface
public Interface Transformador <E, S> {   //no hace falta poner abstract 
    public S transformar (E entrada); 

}

```


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

List<Integer> lista= ...
 lista.forEach(i->{
    if(i>0){
        sout(n+"es positivo); 
        }
    })

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`. 
//VA ENTRAR EN EL EXAMEN 
Referencia a metodo ( : : )-> persona saludar 


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

4 SITUACIONES: 
->Referencia a método estático -> Clase :: metodoEstático
->Referencia a Constructor-> Clase::new
->Referencia a metodo de instancia
    a: Sin instancia conocida -> Clase::metodo (Persona::getNumViajes)(Esto es un BiFuction <Perona, ciudad, Intefer>)
    b:Con instancia conocida -> instancia::metodo (pepe::getNumViajes)(Esto es un Function<Ciudad, Integer>)


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

Collections.sort-> te ordena una coleccion 
Collections.sort (personas, Comparator.comparing(Persona::getEdad)); 
Me crea un comparator que cada vez que reciba Persona las compara por edad 

Collections.sort (personas, Comparator.comparing(Persona::getEdad) thenComparing (Persona::getNombre)); 






________________________________________________________x____________________________________________________________________________
ASPECTOS FUNCIONALES 

Funcional: Las funciones son ciudadanos de 1º clase. Pueden: 
    -Asignadas a variables 
    -Pasadas como parámetro 
    -Recibidas como respuesta 

Expresiones lamba     (parametros)-> {cuerpo}
   -Anónimas 

CLOUSURE 
-Y si el lenguaje es estáticamente tipado  (En java, interfaces funcionales -> una interfaz con "solo un " metodo abstracto)

interface Runable {
    void run (); 
}
    Runable f=()-> Sout("hola")


Interfaces funcionales , les puedo asignar:
    -Una expresion lamnda
    -Una implementacion que programe y le puse nombre ()-> new UnaImplementacionQueProgrameYlePuseNombre(); 

public MiFuncion implements Fuction<Integer, String>{
    String apply (Integer e){....}
}