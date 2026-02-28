<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

Opción 1: Retorno de un "Valor Centinela" (NaN)
Esta técnica consiste en devolver un valor especial que no sea un número real válido para indicar que algo salió mal. En matemáticas, la raíz de un número negativo no es un número real, por lo que devolvemos NaN (Not a Number).

Ventaja: No necesitas variables adicionales en la firma de la función.
Desventaja: El usuario debe acordarse de verificar el resultado con isnan(), o el error se propagará silenciosamente en los cálculos siguientes.

```java 
#include <stdio.h>
#include <math.h>

float raiz(float n) {
    if (n < 0) {
        return NAN; // Valor centinela definido en math.h
    }
    return sqrt(n);
}

int main() {
    float num = -5.0;
    float resultado = raiz(num);

    if (isnan(resultado)) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("El resultado es: %.2f\n", resultado);
    }
    return 0;
}

```

Opción 2: Código de Estado y Puntero de Salida
Este es el estándar en sistemas donde el rendimiento y la claridad son críticos. La función devuelve un int (0 para éxito, otro número para error) y el resultado real se "escribe" en una dirección de memoria proporcionada por el usuario a través de un puntero.

Ventaja: Separa claramente el estado de la ejecución del valor obtenido. E
Desventaja: La sintaxis es un poco más verbosa debido al uso de punteros.

```java
#include <stdio.h>
#include <math.h>

// Retorna 0 si todo ok, -1 si hay error
int raiz_segura(float n, float *resultado) {
    if (n < 0) {
        return -1; // Código de error
    }
    *resultado = sqrt(n);
    return 0; // Éxito
}

int main() {
    float num = -10.0;
    float res;

    if (raiz_segura(num, &res) != 0) {
        printf("Error critico: Entrada invalida (%.2f).\n", num);
    } else {
        printf("La raiz es: %.2f\n", res);
    }
    return 0;
}

```




## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un evento inesperado o una condición de error que ocurre durante la ejecución de un programa y que interrumpe su flujo normal de instrucciones.

A diferencia de los códigos de error en C (donde el error viaja "en la mano" del valor de retorno), una excepción es como una alarma inteligente que salta por encima de las funciones hasta encontrar a alguien que sepa cómo apagarla.

¿Con qué objetivo las usa un programador?
Cuando un programador implementa o llama a una función, utiliza excepciones con tres metas principales:

        1.Separación de preocupaciones (Clean Code):
        Permite separar el código que hace el trabajo sucio (la lógica de negocio) del código que maneja los desastres. Así, no llenas tu algoritmo principal de if (error) { ... } en cada línea.

        2.Propagación Automática:
        Si una función llamada a diez niveles de profundidad falla, la excepción "burbujea" hacia arriba automáticamente. No tienes que pasar el error manualmente de función en función a través de toda la jerarquía de llamadas.

        3.Obligación de manejo (Robustez):
        En muchos lenguajes, si una función puede lanzar una excepción crítica, el programador que la usa está obligado a decidir qué hacer con ella (capturarla o dejarla pasar). Esto evita que los errores pasen desapercibidos y el programa termine en un estado corrupto o "congelado".

En resumen: El objetivo es crear una red de seguridad que capture fallos (como un archivo que no existe o una división por cero) antes de que el programa colapse por completo, permitiendo que la aplicación se recupere o se cierre de forma elegante.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

```java 
public class Calculadora {

    /**
     * Calcula la raíz cuadrada de un número.
     * @param n El número flotante.
     * @return La raíz cuadrada.
     * @throws IllegalArgumentException si el número es negativo.
     */
    public double raiz(double n) {
        if (n < 0) {
            // "Lanzamos" el objeto excepción. La función se detiene aquí.
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo: " + n);
        }
        return Math.sqrt(n);
    }

    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        double numero = -9.0;

        try {
            // Intentamos ejecutar el código que podría fallar
            System.out.println("Calculando...");
            double resultado = calc.raiz(numero);
            
            // Esta línea NO se ejecutará si la línea anterior lanza una excepción
            System.out.println("El resultado es: " + resultado);
            
        } catch (IllegalArgumentException e) {
            // Capturamos el error "desde fuera" de la función
            System.err.println("ERROR capturado en main: " + e.getMessage());
        } finally {
            // Este bloque es opcional y se ejecuta siempre, haya error o no
            System.out.println("Finalizando operación de cálculo.");
        }
    }
}

```


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

1.¿Qué es "lanzar" (throw) una excepción?
        Es el acto de notificar que algo ha salido mal y que la función ya no puede cumplir su promesa. En nuestro ejemplo de Java, cuando raiz(-9.0) detecta el número negativo, "suelta" el testigo y hace sonar una alarma.

        En el código: throw new IllegalArgumentException(...).
        Resultado: La función raiz se detiene inmediatamente. No llega a ejecutar el return.

2.¿Qué es "propagar" una excepción?
        Si la función que lanzó el error no sabe cómo arreglarlo, la excepción viaja hacia atrás por la pila de llamadas (call stack) buscando a alguien que sí sepa.

        En el ejemplo: raiz le pasa el "problema" a main. Si main no tuviera un try-catch, se lo pasaría a la Máquina Virtual de Java (JVM), que finalmente detendría el programa.

3.¿Qué le ocurre a las funciones en la pila de llamadas?
        Aquí está la clave: Las funciones que no controlan la excepción se interrumpen bruscamente. Cuando la excepción se propaga por una función, esta "muere" (se saca de la pila) en ese mismo instante. Todas sus variables locales se destruyen y su ejecución se corta. No hay marcha atrás

4.¿Se reanudan las funciones que no la controlan?
        No. Una vez que una excepción atraviesa una función sin ser capturada, esa función ha terminado para siempre.
        Si raiz lanza la excepción, el código que estaba justo debajo de la llamada a raiz en el main nunca se ejecutará.

        El control del programa salta directamente al bloque catch. Solo después de que el catch (o el finally) termine, el programa sigue su curso normal desde ese punto hacia adelante, pero nunca regresa a donde se quedó la función que falló.

```java
public void calcularYMostrar() {
    double r = calc.raiz(-5); // 1. LANZA la excepción aquí.
    System.out.println(r);    // 2. ESTO NUNCA SE EJECUTA.
}

public static void main(String[] args) {
    try {
        instancia.calcularYMostrar(); // 3. La excepción SE PROPAGA hasta aquí.
    } catch (Exception e) {
        // 4. Aquí se CONTROLA / CAPTURA.
        System.out.println("Error manejado.");
    }
    // 5. El programa SIGUE desde aquí, pero calcularYMostrar() ya desapareció.
}
```
## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La "propagación natural" es, posiblemente, el salto evolutivo más importante entre el manejo de errores "manual" de C y el manejo "estructurado" de Java o C++.

Aquí te detallo las ventajas estratégicas que ofrece frente al modelo de C:

    1.Eliminación del "Efecto Burbujeo" Manual
        En C, si una función a 10 niveles de profundidad encuentra un error, cada una de las 9 funciones intermedias debe
        Recibir el código de error, Comprobarlo con un if, Retornarlo a la función superior.

        Si un solo programador olvida un if en la cadena, el error se "silencia" y el programa sigue operando con datos corruptos. En Java, las funciones intermedias no tienen que hacer nada; la excepción las atraviesa automáticamente hasta encontrar un catch.

    2. Reducción del "Código Espagueti" (Ruido Visual)
        En C, el flujo lógico se mezcla constantemente con el control de errores.  

    3. Contexto Rico de Información (Stack Trace)
        Cuando una excepción se propaga en Java, lleva consigo un objeto que contiene el Stack Trace (la traza de la pila).

        En C, si recibes un -1, a veces es difícil saber qué función exacta de toda la cadena generó el error.
        En Java, la excepción sabe exactamente en qué archivo, qué método y en qué línea de código se originó el problema, y qué camino siguió hasta llegar a ti.

    4.Gestión Centralizada de Errores
        La propagación natural permite que decidas dónde es más inteligente manejar el error.

        No tienes que manejar el error de la raiz dentro de una función matemática intermedia que no sabe qué decirle al usuario.

        Puedes dejar que el error suba hasta la Capa de Interfaz de Usuario, que es el único lugar donde realmente se puede mostrar un mensaje de alerta con sentido o pedir nuevos datos.

    5.Seguridad: "Falla rápido o no falles"
        En C, es fácil ignorar un valor de retorno por accidente.
        En lenguajes con excepciones, si no capturas un error crítico, el programa se detiene (crash).
        Aunque parezca malo, es preferible que un programa se detenga a que siga funcionando con una raíz cuadrada negativa que luego corrompa una base de datos o el sistema de navegación de un avión.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

