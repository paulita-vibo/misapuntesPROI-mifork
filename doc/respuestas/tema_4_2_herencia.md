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
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

***CLASE 
Composición -> "Tiene un/tiene varios" vs. Herencia-> "es un"
1-> Compatibilidad de tipos 
    Soldado s= new Artillero("Pepe"); 

2-> Herencia de estado y comportamiento 
    Todos los atributos de estado estan en Artillero y todos los metodos del comportamiento está en Artillero ç

```java 
    public class Soldado {
        private String nombre; 

        private Soldado (String nombre){
            this.nombre= nombre; 
        }

        public void saludar(){
            sout ("Le saluda el soldado"+nombre); 
        }
    }

    public class Artillero extends Soldado {  //extablecemos la herencia

        public int numCohetes; 

        public Artillero(int numCohetes){
            this.numCohetes=numCohetes; 
        }

        public int getnumCohetes(int numCohetes){
            return this.numCohetes; 
        }
    }

     public class Zapador extends Soldado {  //extablecemos la herencia

        public int numMinas; 

        public Zapador(int numMinas){
            this.numMinas=numMinas; 
        }

        public int getnumCohetes(){
            return this.numMinas; 
        }
    }

    public class PruebaHerencia{
        public static void pasar Revista(Soldado[] soldados){
            for (Soldado soldado:soldados){
                soldado.saludar(); 
            }
        }
        public static void main (String [] args){
            Soldado [] soldadosVarios = new Soldado [3]; 
            soldadosVarios [0]= new Artillero ("Juan", 3); 
            soldadosVarios[1] = new Artillero ("Ana", 1); 
            soldadosVarios [2]= new Zapador ("Pepe", 10); 

            pasarRevista(soldadosVarios); //metodo que se ha quedado cerrado independientemente de que se extienda el array de soldado -> gracias que vive abstraido 
        }
    }



```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Se ejecutan un constructor por cada clase de la jerarquia y se ejecutan de arriba a abajo. Super indica como debe invocarse el supercontrustor desde la subclase 
SI, si tu clase base a solo tiene el constructor con algun parametro 

```java 
  public class Artillero extends Soldado {  //extablecemos la herencia

        public int numCohetes; 

        public Artillero(int numCohetes, String nombre){
            //Nos obliga a llamar al superConstructor 
            super(nombre, )
            this.numCohetes=numCohetes; 
        }

        public int getnumCohetes(){
            return this.numCohetes; 
        }
    }
```

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

1. ¿forman parte de una instancia de la subclase en memoria?
Sí. 

2. ¿Los atributos privados de la superclase están en la subclase?

Sí.

Cuando creas un objeto de una subclase, en memoria se construye un único objeto que contiene:

La parte de la superclase
La parte de la subclase

Es decir, la subclase hereda la estructura en memoria, aunque no todo sea accesible.

EJEMPLO: 
```java 
class Soldado {
    private String nombre;
    private int edad;

    public Soldado(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
}

class SoldadoRaso extends Soldado {
    private String rango;

    public SoldadoRaso(String nombre, int edad, String rango) {
        super(nombre, edad);
        this.rango = rango;
    }
}
```
3. ¿Se pueden usar directamente desde la subclase?
No, no se pueden acceder directamente.

El modificador private significa:
Solo accesible dentro de la propia clase Soldado
Ni siquiera las subclases pueden acceder directamente

```java 
class SoldadoRaso extends Soldado {

    public void mostrar() {
        System.out.println(nombre); // ERROR
    }
}
```




## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.4
¿Qué implica para la extensibilidad?

Que puedes:
Añadir nuevas subclases de Soldadoç
Sin modificar el código existente que trabaja con Soldado

Esto es exactamente el principio “abierto/cerrado”:
Abierto a extensión (puedes añadir nuevas clases)
Cerrado a modificación (no tienes que tocar código ya hecho)

```java
    abstract class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public abstract void saludar();
}

class Artillero extends Soldado {
    private int nivel;

    public Artillero(String nombre, int nivel) {
        super(nombre);
        this.nivel = nivel;
    }

    public void saludar() {
        System.out.println("Soy artillero " + nombre);
    }
}

class Artillero extends Soldado {
    private int nivel;

    public Artillero(String nombre, int nivel) {
        super(nombre);
        this.nivel = nivel;
    }

    public void saludar() {
        System.out.println("Soy artillero " + nombre);
    }
}

class Medico extends Soldado {
    private int experiencia;

    public Medico(String nombre, int experiencia) {
        super(nombre);
        this.experiencia = experiencia;
    }

    public void saludar() {
        System.out.println("Soy médico " + nombre);
    }
}

public class PruebaHerencia {

    public static void pasarRevista(Soldado[] soldados){
        for (Soldado soldado : soldados){
            soldado.saludar(); // polimorfismo
        }
    }

    public static void main(String[] args){
        Soldado[] soldadosVarios = new Soldado[4];

        soldadosVarios[0] = new Artillero("Juan", 3);
        soldadosVarios[1] = new Artillero("Ana", 1);
        soldadosVarios[2] = new Medico("Pepe", 10);
        soldadosVarios[3] = new Francotirador("Luis", 95); // NUEVO

        pasarRevista(soldadosVarios);
    }
}
```


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

```java
    public static void main(String[] args){
        Soldado[] soldadosVarios = new Soldado[4];

        soldadosVarios[0] = new Artillero("Juan", 3);
        soldadosVarios[1] = new Artillero("Ana", 1);
        soldadosVarios[2] = new Medico("Pepe", 10);
        soldadosVarios[3] = new Francotirador("Luis", 95); // NUEVO

        pasarRevista(soldadosVarios);
        Soldado soldado = new Artillero ("juan", 10); //upcasting, es automatico (implicito)

        if (soldado instanceof Artillero){  //downcasting automatico 
            Artillero comoArtillero= (Artillero soldado) ;//downcasting, explicitco (eso que va entre parentesis) 
            int cohetes = comoArtillero.getnumCohetes(); 
            sout ("cohetes"+cohetes); 
        }

        Medico medico = (Medico)soldado; //downcasting, explicitco (eso que va entre parentesis) 
    }
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

```java 
class Soldado {
    protected String nombre; // ahora es accesible por las subclases

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

class Zapador extends Soldado {

    public Zapador(String nombre) {
        super(nombre);
    }

    public void ponerBomba() {
        // acceso directo al atributo protegido
        System.out.println(nombre + " está colocando una bomba");
    }
}

public class Prueba {
    public static void main(String[] args) {
        Zapador z = new Zapador("Carlos");
        z.ponerBomba();
    }
}
```




## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?
tados a objetos existe una única clase base universal, aunque es una idea bastante común.

En general:

Algunos lenguajes sí definen una clase raíz de la que heredan todos los objetos.
Otros no tienen una jerarquía única (por ejemplo, permiten varios tipos base o incluso modelos sin clases estrictas).

Ejemplos:

En lenguajes como Java o C#, sí existe una clase base común.
En otros como C++, no es obligatorio que todas las clases hereden de una clase raíz única.

¿Qué ocurre en Java?
En Java:

Existe una clase base universal llamada Object.
Todas las clases heredan directa o indirectamente de Object, incluso si no lo declaras explícitamente.
Esto significa que todos los objetos comparten ciertos métodos comunes como toString(), equals(), hashCode(), etc.

***Clase 
c++ no tiene una clase base implícita 
Java sí la tiene : Object


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?
La herencia múltiple es una característica de algunos lenguajes orientados a objetos que permite que una clase herede de más de una clase base al mismo tiempo. Es decir, una clase puede adquirir atributos y métodos de varias “clases padre”.

Ejemplo conceptual:
Una clase Híbrido podría heredar de Coche y de Barco, combinando comportamientos de ambos.

¿En qué lenguajes existe?
Sí existe en lenguajes como C++.
No todos los lenguajes la permiten directamente, porque puede generar conflictos (por ejemplo, el famoso “problema del diamante”).

***CLASE 
Herencia multiple-> Heredar de mas de una clase 
No existe en Java la herencia multiple 


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

***clase 
```java 
public class UsuarioNoEcontradoException extends RuntimeException{
    public UsuarioNoEncontradoException (String mensaje, Throwable causa, Usuario usuarioNoencontrado){
        super (mensaje, causa); 
        this.usuarioNoEncontrado = usuarioNoEncontrado; 
    }

    public UsuarioNoEncontradoException (String mensaje, Usuario usuarioNoencontrado){
        super (mensaje); 
        this.usuarioNoEncontrado = usuarioNoEncontrado; 
    }
     public Usuario getUsuarioNoEncontrado(){
        return this.usuarioNoEncontrado; 
     }
    }

    /* Usuario UsuarioABuscar=...

        //throw new 
``` 


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?
La idea de fondo es correcta: la herencia no debería usarse solo para reutilizar código.

🔹 Diferencia clave

Herencia (is-a)
Una clase hija es un tipo de la clase padre.
Ejemplo: Perro es un Animal.

Composición (has-a)
Una clase contiene otra para usar su funcionalidad.
Ejemplo: Coche tiene un Motor.

🔹 ¿Por qué no usar herencia solo para reutilizar código?

Porque la herencia implica algo más fuerte que “copiar comportamiento”:

Crea una relación conceptual rígida (jerarquía).
Acopla fuertemente las clases (cambios en la base afectan a las hijas).
Puede romper el diseño si la relación “es-un” no es real.
Puede violar el principio de sustitución (Liskov) si se usa mal.

👉 Ejemplo problemático:
Hacer que Empleado herede de ArchivoPDF solo para reutilizar un método de impresión. No tiene sentido conceptual.

Cuándo usar cada una

Usa herencia cuando:

Existe una relación clara de tipo (“es-un”).
Quieres especializar comportamiento.
La jerarquía tiene sentido semántico.

Usa composición cuando:

Solo quieres reutilizar funcionalidad.
Necesitas flexibilidad (cambiar comportamientos en tiempo de ejecución).
No hay una relación “es-un”.

🔹 En Java

Esto es especialmente importante porque:

Solo hay herencia simple de clases.
Se fomenta el uso de interfaces + composición.

Ejemplo típico con composición:
```java 
class Motor {
    void arrancar() { }
}

class Coche {
    private Motor motor = new Motor(); // composición

    void arrancar() {
        motor.arrancar();
    }
}
```



***CLASE 

->No usar la Herencia solo por reutilizar el codigo 
    Debe usarse cuando se necesita la compatibilidad de tipos 

->Usar herencia implica un fuerte acoplamiento desde la clase derivada hacia la clase base 
    -> La clase derivada depende mucho de la base 
    -> Cambios internos en la clase base base podrían llegar a afectar a las derivadas 

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

La recomendación de “favorecer la composición frente a la herencia” no significa “no usar herencia”, sino usar composición por defecto y herencia solo cuando encaja de verdad.

¿Por qué se prefiere la composición?

1. Menor acoplamiento
Con herencia, la clase hija queda muy ligada a la clase padre (cualquier cambio puede romperla).
Con composición, solo dependes de lo que usas, no de toda la jerarquía.

2. Más flexibilidad
La composición permite cambiar comportamientos en tiempo de ejecución:

Puedes sustituir componentes.
Puedes combinar funcionalidades de distintas formas.

3. Evita jerarquías artificiales
La herencia obliga a una relación “es-un”.
Si esa relación no es totalmente cierta, el diseño se vuelve confuso y frágil.

4. Reduce problemas clásicos de la herencia
Como:

El “problema del diamante”.
Dificultad para reutilizar partes sin heredar todo.
Violaciones del principio de sustitución (Liskov).

5. Reutilización más controlada
Con composición reutilizas exactamente lo que necesitas, no todo lo que heredas.

Ejemplo intuitivo

Herencia (menos flexible):
```java 
class Ave {
    void volar() { }
}

class Pinguino extends Ave { // ❌ Problema conceptual
    // no debería volar
}
```
Composición (más flexible):
```java 
class ComportamientoVuelo {
    void volar() { }
}

class Ave {
    private ComportamientoVuelo vuelo;
}
```
🔹 En Java

Esto cobra aún más sentido porque:

Solo hay herencia simple.
Se promueve el uso de interfaces + composición para construir sistemas más modulares.




## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Cuando se dice que “la herencia rompe la encapsulación”, se refiere a que una clase hija queda demasiado expuesta a los detalles internos de la clase padre, perdiendo el aislamiento que debería proporcionar la encapsulación.

🔹 ¿Qué es la encapsulación?

La encapsulación implica que:

Una clase oculta su implementación interna.
Solo expone una interfaz pública bien definida.
El resto del código no depende de cómo está hecha por dentro.
🔹 ¿Qué ocurre con la herencia?

Con herencia, la clase hija:

Depende de la implementación interna de la clase padre (no solo de su interfaz).
Puede acceder a elementos protected o comportamientos internos.
Puede verse afectada por cambios internos del padre, incluso si su interfaz pública no cambia.

👉 Es decir, la clase padre deja de estar completamente “encapsulada”.

🔹 Ejemplo típico

En Java:
```java 
class Lista {
    protected int tamaño;

    void añadirElemento() {
        tamaño++;
    }
}

class ListaControlada extends Lista {
    void resetear() {
        tamaño = 0; // accede directamente al estado interno
    }
}
```
Aquí:

ListaControlada depende directamente de cómo Lista gestiona su estado.
Si la implementación de Lista cambia, la clase hija puede romperse.

🔹 Problema clave
La herencia crea una relación en la que:
La clase hija no solo usa la clase padre.
Conoce y depende de sus detalles internos.

Esto rompe el principio de:

“una clase debería poder cambiar su implementación sin afectar a otras”.

🔹 ¿Cómo lo evita la composición?
Con composición:

Solo interactúas con la interfaz pública del objeto contenido.
No accedes a su estado interno.
El acoplamiento es mucho menor.

✔️ Conclusión
Decir que “la herencia rompe la encapsulación” significa que:

La implementación interna deja de estar realmente oculta.
Las clases hijas quedan fuertemente acopladas a los detalles del padre.
Esto hace el sistema más frágil ante cambios.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

1º
```java 
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}
class Estudiante extends Persona {
    private String carrera;

    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }
}
class Trabajador extends Persona {
    private String empresa;

    public Trabajador(String dni, String nombre, String empresa) {
        super(dni, nombre);
        this.empresa = empresa;
    }
}
´´´

2º
```java 
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}
class Estudiante {
    private DatosPersonales datos;
    private String carrera;

    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }
}
class Trabajador {
    private DatosPersonales datos;
    private String empresa;

    public Trabajador(DatosPersonales datos, String empresa) {
        this.datos = datos;
        this.empresa = empresa;
    }
}
```
👉 Aquí:

Estudiante tiene datos personales
Trabajador tiene datos personales
🔹 Diferencia conceptual clave
Herencia → relación “es-un”
(Estudiante es una persona)

Composición → relación “tiene-un”
(Estudiante tiene datos personales)

