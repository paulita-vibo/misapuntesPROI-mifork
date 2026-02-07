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



## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de un objeto o clase es el conjunto de métodos accesibles desde fuera de la clase, es decir, aquello que otros objetos pueden usar para interactuar con ella 

La ocultación de información se apoya directamente en la interfaz pública: 
-La interfaz pública expone solo lo necesario (public)
-Los detalles internos de implementación (atributos y métodos private) quedan ocultos 
-El usuario de la clase no puede acceder ni depender del estado interno, solo usar la interfaz 


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Porque es la parte visible y estable del contrato con el resto del sistema. Todo el código que use esa clase dependerá directamente de ella 

No es facil cambiar la interfaz pública ya que rompe dependencias, propaga cambios, afecta a la reutilización y compatibilidad y puede producir errores sutiles 

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clases son condiciones que deben cumplirse siempre para que un  objeto esté en un estado válido, antes y despues de ejecutar cualquier método público de la clase. Describen las reglas internas que definen la coherencia del objeto 

La ocultación de información es clave porque impide modificaciones directas del estado interno, centraliza el control del estado, facilita la vadilación y reduce errores y estados inconscientes



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?




## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

El uso de modificadores de acceso public y private depende del nivel de estructura. 
public: La clase es accesible desde cualquier otra clase en cualquier paquete 
private: No se puede aplicar a clases de nivel superior 


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


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
