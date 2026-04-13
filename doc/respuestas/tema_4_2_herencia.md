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

### Respuesta


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
