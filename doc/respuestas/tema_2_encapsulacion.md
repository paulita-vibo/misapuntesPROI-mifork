<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

En Programación Orientada a Objetos tanto la encapsulación como la ocultación de información buscan proteger y organizar mejor los datos y el comportamiento de los objetos, pero cada concepto pone el foco en algo distinto: 
La encapsulacion consiste en agrupar los datos (atributos) y los métodos que operan sobre ellos dentro de una clase, y controlar el acesso a esos datos mediante modificadores (como privare, protectec o public)
La ocultación de información busca esconder los detalles internos de implementación de un objeto, mostrando solo lo necesario a través de una interfaz pública. 
Alguna de sus ventajas son: 
-Reduce el acoplamiento
-Mejora la mantenibilidad 
-Aumenta la seguridad 
-Facilita la reutilización 
-Mejora la comprensión del código 

****CLASE
 La encapsulacion tiene que ver con "Protección": 
    -Evito estados no validos de mis objetos 
    -Evito dependencias desde fuera que no quiero 

    2 Partes-> He juntado estado y comportamiento en un artefacto (la clase), y ahora puedo ocultar ciertas partes del exterior 



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de un objeto o clase es el conjunto de métodos accesibles desde fuera de la clase, es decir, aquello que otros objetos pueden usar para interactuar con ella 

La ocultación de información se apoya directamente en la interfaz pública: 
-La interfaz pública expone solo lo necesario (public)
-Los detalles internos de implementación (atributos y métodos private) quedan ocultos 
-El usuario de la clase no puede acceder ni depender del estado interno, solo usar la interfaz 

***CLASE 
 Interfaz pública: Los miembros (atributos/metodos indestinguidamente) que se ven desde fuera, es decir, los que no están ocultos 


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Porque es la parte visible y estable del contrato con el resto del sistema. Todo el código que use esa clase dependerá directamente de ella 

No es facil cambiar la interfaz pública ya que rompe dependencias, propaga cambios, afecta a la reutilización y compatibilidad y puede producir errores sutiles 

***CLASE 
La interfaz pública si se cambia tiene más consecuencias que cualquier cambio en la parte oculta 

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clases son condiciones que deben cumplirse siempre para que un  objeto esté en un estado válido (sea mutable),  antes y despues de ejecutar cualquier método público de la clase. Describen las reglas internas que definen la coherencia del objeto 

La ocultación de información es clave porque impide modificaciones directas del estado interno, centraliza el control del estado, facilita la vadilación y reduce errores y estados inconscientes

***CLASE 
INVARIANTES DE CLASE-> Condiciones que los objetos de esa clase cumplen o deben cumplir para ser válidos y durante toda la vida del objeto 
    Ej: Cuenta bancaria debe tener siempre saldo positivo (invariante de clase)
        Persona debe tener edad>=0
        Rectángulo debe tener ancho y alto>0

    ¿Es una invariante de clase decir que una variable tiene que ser un numero entero? Si, sistema de tipos ?????????????


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?
```java 
    class Punto {                       //Ahora mismo tengo garantizado que una vez se crea no va cambiar el valor de sus coordenadas
        private double x; 
        private double y; 
        
        public Punto (double x, double y){ //Interfaz publica 
            this.x=x; 
            this.y=y; 
        }

        double distanciaAOrigen(){         //Si no le pongo nada es visible pero desde clases del mismo paquete , tb seria interfaz publica pero
            return Math.sqrt(this.x*this.x+this.y*this.y);                                                      //solo para los de mi paquete 
        }

    }

``` 



## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

El uso de modificadores de acceso public y private depende del nivel de estructura. 
public: La clase es accesible desde cualquier otra clase en cualquier paquete 
private: No se puede aplicar a clases de nivel superior 

***CLASE 
En java 
    LPublic:clases, atributos y métodos 
    Lprivate: clases internas (nolos estamos viendo), atributos y métodos 


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Java es muy específico y ofrece cuatro niveles de visibilidad: 
-public
-protected
-default
-private
 Aunque solo usas tres palabras clave, el cuarto nivel es el que queda cuando no escribes nada 

Cada lenguaje tiene su propia "personalidad" para manejar la privacidad: 
-C# añade niveles mas granulares como internal, protected internal o private protected 
-Python no tiene restricciones reales. Todo es público técnicamente, pero se usan convenciones de nombres (_variable, _ _ variable) 
-JavaScript usa el símbolo # antes del nombre de la variable para marcarla como privada de forma nativa 

***CLASE 
En java 
    -protected, solo se ve desde "subclases" (las veremos en el tema de herencia)
    -"packaged-private" o sin modificador, solo se ve desde el paquete 


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

***CLASE 
I¿Para qn esta oculto para las mismas clases o para las instancis del objeto? 
```java 
    class Punto {                       //Ahora mismo tengo garantizado que una vez se crea no va cambiar el valor de sus coordenadas
        private double x; 
        private double y; 
        
        public Punto (double x, double y){ //Interfaz publica 
            this.x=x; 
            this.y=y; 
        }

        public double distanciaAOrigen(){         
            return Math.sqrt(this.x*this.x+this.y*this.y);                                                     
        }

        public double distaciaAOtroPunto (Punto otro){
            double dx =this.x-otro.x; 
            double dy=this.y-otro.y; 
            return Math.sqrt(dx*dx+dy*dy); 
        }

    }

``` 
¿Esto complila? -> Si 
La a, está oculta para código de otras clases 


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

En programación orientada a objetos, los getter y los setter son métodos que sirven para acceder y modificar los atributos (variables) de un objeto de gorma controlada
Getter-> obtiene el valor de un atributo 
Setter-> cambia el valor de un atributo 

***CLASE 
"getter" y "setter" permiten dar acceso a atributos privados para obtener su valor o cambiarlo 
```java 
    class Punto {                      
        private double x; 
        private double y; 
        
        public Punto (double x, double y){ 
            this.x=x; 
            this.y=y; 
        }

        public double distanciaAOrigen(){         
            return Math.sqrt(this.x*this.x+this.y*this.y);                                                     
        }

        public double distaciaAOtroPunto (Punto otro){
            double dx =this.x-otro.x; 
            double dy=this.y-otro.y; 
            return Math.sqrt(dx*dx+dy*dy); 
        }
        public double get(){
            return this.x; 
        }
        public void setX(double x){  //Para modificar el valor recibe el valor al que lo quiero cambiar y luego lo devuelvo (return)
            this.x=x; 
        }
        public String getNombre(){   //Los String se pasan por copia de la referencia
            return this.nombre; 
        }

    }

``` 



## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, se refiere a que mejora la seguridad, se habla de seguridad lógica y estructural, no de ciberseguridad. Es decir, proteger el estado interno del objeto de usos incorrectos dentro del propio programa

***CLASE 
No, esto no es ciberseguridad, es facilitar una programación con menos buggs 

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

***CLASE 
Un miembro de clase no está asociado a ninguna instancia si no que; es compartido por todas las instancias 
    En métodos, no hay this 
Un miembro de instancia está asociado a cada instancia; no son compartidos 


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Si, tiene sentido, pero solo en casos concretos, un constructor privado impide que otras clases creen instancias libremente. Eso se usa cuando quieres controlar cómo y cuántos objetos existen

***CLASE 
Tiene sentido? A veces 
    -Un constructor auxiliar oculto,llamado desde otros constructores públicos 
    -Cuando prefiero usar métodos factoría (inicializador statico)
        -Cuando quiero controlar el nº de instancias 
    

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

***CLASE 
Con static 


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

***CLASE 
```java 
    class Punto {                      
        private double x; 
        private double y; 
        
        public Punto (double x, double y){ 
            this.x=x; 
            this.y=y; 
        }
        //SI QUEREMOS OCULTAR EL CONSTRUCTOR//
        public static Punto nuevoEn (double x, double y){
            return new Punto (x,y); 

        }
        ////////////////////
        /// 
            //METODO FACTORIA///////////////////////////////////////////////////////////////////////////////////////////////////////
        public static Punto puntoRedondeado(double x, double y){
           return new Punto (Math.round(x), Math.round(y));                              

        }
            ////////////////////////////////////////

        public double distanciaAOrigen(){         
            return Math.sqrt(this.x*this.x+this.y*this.y);                                                     
        }

        public double distaciaAOtroPunto (Punto otro){
            double dx =this.x-otro.x; 
            double dy=this.y-otro.y; 
            return Math.sqrt(dx*dx+dy*dy); 
        }
        public double get(){
            return this.x; 
        }
        public void setX(double x){  
            this.x=x; 
        }
        public String getNombre(){  
            return this.nombre; 
        }

    }

    class EjercicioEncapsulacion{
        public static void main (String []args){
            Punto p =Punto.puntoRedondeado(4.5,6.7); 
            Punto p2=Punto.en(2,3); 
        }
    }

``` 



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

***CLASE 
```java 
    class Punto {
        private double x; 
        private double y; 

        private double []coordenadas = new double [2]; 
        
        public Punto (double x, double y){ 
            this.coordenadas[0]=x; 
            this.coodenadas[1]=y; 
        }

         public double getX(){                  //Son primitivas, trabajan directamente con la base interna
            return this.coordenadas[0];

         }

         public double getY(){
            return this.coordenadas [1]; 
         }
       
       //-------------------------------- Arriba primitivas, Abajo derivadas o no primitivas (estan programadas en base a las primitivas)

        public double distanciaAOrigen(){         
            return Math.sqrt(this.getX()*this.getX()+this.getY()*this.getY());                                                     
        }

        public double distaciaAOtroPunto (Punto otro){
            double dx =this.getX()-otro.getX(); 
            double dy=this.getY()-otro.getY(); 
            return Math.sqrt(dx*dx+dy*dy); 
        }
       
        class EjercicioEncapsulacion{
        public static void main (String []args){
            Punto p =new Punto(4,5); 
            System.out.println("Tu punto está en: "+p.)      ?????????????????????
            System.out.println (p.distanciaAOrigen()); 
        }

    }  
    }              
``` 


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clases? 

Si haces el atributo público y además pones getters/setters públaicos, en la paractiva no ganas nada: cualquiera puede saltarse los métodos y tocar el estado directamente 
La clave es que el getter/setter no solo es un "acceso", es un punto de control 

```java
private int edad;

public void setEdad(int edad) {
    if (edad < 0) {
        throw new IllegalArgumentException("La edad no puede ser negativa");
    }
    this.edad = edad;
}
```
Si edad fuera pública, este control sería imposible de garantizar 

La conveción más extendida es: atributos provados, acceso a través de métodos 
Eso se suele resumir como: encapsulación, ocultar estado y programar contra interfaces, no contra immplementación

Sí, esta totalmente relacionado con las variantes de clases. Estas son reglas que siempre deben cumplirse. Si los atributos son púublicos cualquier código externo puede romper esas invariantes y la clase deja de ser dueña de su propio estado, sin embargo, si los atributos son privados la clase controla cuándo y cómo cambia su estado y las invariantges se validan en constructores y métodos 


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

1.¿Que significa que una clase sea "inmutable"? 

Una clase es inmutable cuando una vez creado un objeto, su estado no puede cambiar nunca. Es decir, sus atributos no se modifican después del contructor y cualquier "cambio" produce un nuevo onjeto, no modifica el existente 

```java 
public final class Dinero {
    private final BigDecimal importe;
    private final String moneda;

    public Dinero(BigDecimal importe, String moneda) {
        this.importe = importe;
        this.moneda = moneda;
    }
}
``` 
Una vez creado, en el main: :
```java 
Dinero d = new Dinero(100, "EUR");
// no hay forma de cambiar importe ni moneda
```

Entonces, como "cambiarias" algo?
Creando otro objeto: 
```java 
public Dinero sumar(BigDecimal cantidad) {
    return new Dinero(this.importe.add(cantidad), this.moneda);
}
```
2.¿Qué es un método modificador? 

Un método modificador es cualquier método que cambia el estado interno del objeto 

3.¿Un método modificador es siempre un setter? 

No, un setter es solo un tipo concreto de método modificador 

4.¿Tiene ventajas que una clase sea inmutable? 
Si: 
-Invariantes garantizadas 
-Mucho más facil de razonar 
-Thrad-safe gratis 
-Menos efectos colaterales 
-Perfectas como value objects 


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

En general no, solo es recomendable incluirlos cuando tengan sentido. Tener setters siempre y como conveción suele ser una  mala señal de diseño: 
-Rompen la encapsulación 
-Convierten las clases en un "struct con métodos" 
-Hacen los objetos frágiles 

Tiene sentido añadir un setter cuando el cambio es simple, no rompe invariantes y representa algo natural del dominio 



## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

1.¿Cómo se comparan objetos de una misma clase en POO? ¿Por su contenido o por su identidad? 

Depende de qué quieras comparar: 
-Por indentidad: Compara si son el mismo objeto en memoria,es decir, si dos referencias apuntan exactamente al mismo objeto (==)  
-Por contenido (estado): Compara si los objetos tiene los mismos valores en sus atributos,aunque sean objetos distintos en memoria (equals())

2.¿Qué es el metodo equals en Java? 

equals es un método de la clase object, que todas las clases heredan. Su propósito es comparar objetos lógicamente, no por referencia  

3.¿Cómo se deben comparar dos cadenas en Java? 

Las cadenas en Javan se comparan con equals, no con ==

```java 
if (s1.equals(s2)) { ... }
```


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que envuelven en un tipo de dato primitivo para poder tratarlo como un objeto dentro de un lenguaje orientado a objetos.Permiten usar valores simples (como int, float, chat) allí donde se acepta objetos 

¿Cómo se hace? 
Se pueden crear wrappers propios para encapsular otros tipos de datos o crear uno nuevo 
```java 
Integer n = new Integer(10);     // forma antigua
Integer m = Integer.valueOf(10); // forma recomendada
```

¿Es un proceso automático? 
Puede serlo o no, según el lenguaje 

En Java existe:
Autoboxing: primitivo → wrapper
Unboxing: wrapper → primitivo

```java 
Integer a = 5;  // autoboxing (int → Integer)
int b = a;      // unboxing (Integer → int)
```
Aquí el programador no hace nada explícito, el compilador lo maneja 

¿Que ventajas tiene las clases wrapper? 
-Permiten usar primitivos como objetos 
-Proveen métodos utiles 
-Permiten trabajar con valores nulos 
-Compatibilidad con APIs orientadas a objetos 

¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?
No. Phyton, Ruby, Smalltalk no tienen primitivos y C++ tiene primitivos pero con un efoque distitno 
Es decir, los wrappers existen porque algunso lenguajes mezclan eficiencia (primitivos) con POO (objetos)


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java? 

En POO, un tipo de dato enumerado es un tipo especial que: 
-Define un conjunto finito y cerrado de valores posibles
-Cada valor representa una opción válida del dominio 
-Evita valores inválidos, mágicos o inconscientes 

```java 
DíaSemana = {LUNES, MARTES, MIÉRCOLES, ...}
EstadoPedido = {CREADO, ENVIADO, ENTREGADO}
```
En vez de usar int, String o constantes sueltas, el dato enumerado modela explícitamente el concepto 

En Java, un enum es una clase. 
Ventajas: 
-Encapsulan comportamiento, no solo datos 
-Control total sobre los valores válidos 
-Evitan el uso de constantes públicas dispersas 
-Seguridad de tipos 
-Encapsulan lógica relacionada al estado 


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
