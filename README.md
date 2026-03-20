# Código Base
    public static long factorial(int n) {
    if (n <= 1) {
    return 1;
    }
    return n * factorial(n - 1);
    }


## TAREAS
1. Sin ejecutar el código, dibuje el árbol o la cadena de llamadas para factorial (5) e indique qué información queda pendiente en cada nivel de la pila.

factorial(5)        ----> (Pendiente: 5 * factorial(4))
    |_____factorial(4)      ----> (Pendiente: 4 * factorial(3))
            |_____factorial(3)      ----> (Pendiente: 3 * factorial(2))
                    |_____factorial(2)      ----> (Pendiente: 2 * factorial(1))
                            |_____factorial(1)      ----> (Pendiente: 1, caso base alcanzado)
                                    |_____1 (caso base) 
                            
                        |_____2 * 1 = 2
                |_____3 * 2 = 6
        |_____4 * 6 = 24
|_____5 * 24 = 120

    Como se puede observar, no se resuelve el problema hasta que se alcanza el caso base. A medida que se resuelven las llamadas recursivas, se van resolviendo las operaciones pendientes en cada nivel de la pila, hasta llegar al resultado final de factorial(5) = 120.

2. Explique por qué esta función no genera un árbol ramificado como Fibonacci, sino una sola rama de
profundidad n.

    Esta función no genera un arbol ramificado como Fibonacci debido a que cada llamada recursiva solo hace una llamada adicional a sí misma. Si vemos el caso de Fibonacci, este hace dos llamadas recursivas, lo que genera un árbol con múltiples ramas. En cambio, en el caso de factorial, cada llamada solo depende de una llamada anterior, lo que resulta en una sola rama.

3. Escriba la recurrencia temporal T(n) y justifique, sin usar frases vacías, por qué su complejidad temporal es lineal y su complejidad espacial también es lineal

    Según lo visto en la clase, la recurrencia temporal T(n) para la función factorial se puede expresar como:
    
    T(n) = T(n - 1) + O(1) para n > 1
    T(1) = O(1)

    Complejidad temporal:
    Aquí la función factorial es lineal ya que se llama a si misma con numeros cada vez más pequeños hasta llegar a 1 o al caso base. Consecuentemente, cada llamada espera la respuesta de la siguiente llamada antes de multiplicar y poder seguir. Por ese mismo motivo no hay muchas ramas, es una sola forma de cadena pero de llamadas.

    Complejidad espacial:
    En este caso, la complejidad espacial también es lineal ya que, cada vez que la función se llama a sí misma, el programa guarda dónde iba y qué valores usaba como en una cajita de la pila. Como las llamadas no se resuelven hasta llegar a 1, puede haber varias cajitas al mismo tiempo. Por eso la memoria usada crece más o menos como n, es decir O(n).

4. Reescriba el algoritmo en una versión iterativa y compare formalmente qué costo desaparece al eliminar la recursión.

    Considero que puede ser algo como lo siguiente:

    public static long factorialIterativo(int n) {
        long resultado = 1;
        for (int i = 2; i <= n; i++) {
            resultado *= i;
        }
        return resultado;
    }

    Como se puede ver, esta versión aunque se puede ver que es más larga, esta no tiene llamadas recursivas, sino que utiliza un bucle para calcular el factorial. Lo que lo hace que sea mas seguro ya que se elimina la recursión. Es decir, no hay riesgo de que se desborde si es que n es muy grande, y así reduce la complejidad espacial.

5. Diseñe una versión de recursividad de cola con acumulador y argumente por qué en Java sigue sin garantizarse una optimización real del stack.

    Considero que se puede hacer algo como lo siguiente:

        long factorialCola(int n, long acumulador) {
            if (n <= 1) {
                return acumulador;
            }
            return factorialCola(n - 1, n * acumulador);
        }

    En este caso vemos que el acumulador se va actualizando en cada llamada recursiva, y la funcion se vuelve a llamar al final. Pero en Java no se garantiza que sea una buena optimización, ya que Java en cada llamada recursiva aún consume espacio en pila, lo que puede lograr que en valores mmuy grandes se desborde. 

6. Explique en qué contexto práctico seguiría prefiriendo la versión recursiva, aun sabiendo que la iterativa usa menos memoria.

    En contextos donde la claridad del código y la facilidad de mantenimiento son prioritarias sobre el uso de memoria, podría preferirse la versión recursiva. Además, en situaciones donde el tamaño de los datos no es extremadamente grande, pienso que la diferencia del uso de cada una en cuestión de memoria es muy significativa.


## Ejercicio 2 — Fibonacci: detectar el verdadero origen del costo (25 puntos)
## Código base:
    public static int fib(int n) {
        if (n <= 1) {
            return n;
        }
        return fib(n - 1) + fib(n - 2);
    }


7. Construya manualmente el árbol de llamadas de fib(6). No se acepta solo el resultado final; debe marcar cuáles subproblemas se repiten.

    fib(6)
    |_____fib(5)
    |     |_____fib(4)
    |     |     |_____fib(3)
    |     |     |     |_____fib(2)
    |     |     |     |     |_____fib(1)
    |     |     |     |     |_____fib(0)
    |     |     |     |_____fib(1)
    |     |     |     
    |     |     |_____fib(2)
    |     |           |_____fib(1)
    |     |           |_____fib(0)
    |     |_____fib(3)
    |           |_____fib(2)
    |           |     |_____fib(1)
    |           |     |_____fib(0)
    |           |_____fib(1)
    |
    |_____fib(4)
            |_____fib(3)
            |     |_____fib(2)
            |     |     |_____fib(1)
            |     |     |_____fib(0)
            |     |_____fib(1)
            |
            |_____fib(2)
                    |_____fib(1)
                    |_____fib(0)

    Se repiten los siguientes subproblemas:
    - fib(4) se repite 2 veces
    - fib(3) se repite 3 veces
    - fib(2) se repite 5 veces
    - fib(1) se repite 8 veces
    - fib(0) se repite 5 veces

8. Explique por qué el costo de este algoritmo no crece por tener recursión, sino por recalcular subproblemas ya resueltos.
    
    El costo de este algoritmo crece no por tener recursión, como se indica en la pregunta, sino por recalcular subproblemas ya resueltos. En el caso especial de Fibonacci, cada llamada hace dos llamadas recursivas, lo que genera un arbol de muchas llamadas. A medida que se profundiza en el árbol, se van repitiendo los mismos subproblemas una y otra vez, lo que hace que el número de llamadas crezca exponencialmente. 

9. Señale al menos tres llamadas redundantes dentro de su árbol y explique cómo esas repeticiones empujan el tiempo hacia crecimiento exponencial.

    Tres llamadas redundantes dentro del árbol de fib(6) son:
    - fib(4) se llama 2 veces
    - fib(3) se llama 3 veces
    - fib(2) se llama 5 veces
    Estas repeticiones empujan el tiempo hacia crecimiento exponencial porque cada llamada a fib(n) genera dos llamadas adicionales a fib(n-1) y fib(n-2). A medida que n aumenta, el número de llamadas se duplica aproximadamente, lo que resulta en un crecimiento exponencial del número de llamadas.

10. Analice cómo cambia la complejidad temporal y espacial cuando pasa de la versión recursiva ingenua a la memorizada (ver teoría).

    Al pasar de la versión recursiva a la memorizada, la complejidad temporal cambia de exponencial a lineal. Esto se debe a que en la versión memorizada, cada subproblema se resuelve una sola vez y su resultado se almacena para futuras referencias. Por lo tanto, en lugar de recalcular los mismos subproblemas múltiples veces, el algoritmo simplemente recupera el resultado almacenado, lo que reduce el gran medida el número de llamadas.

11. Explique por qué la tabulación iterativa en este caso no solo cambia la implementación, sino también la forma de pensar el problema: de arriba hacia abajo vs. de abajo hacia arriba.

    En tabulación iterativa se empieza por los casos base y se va llenando una tabla hacia adelante. En recursión se empieza por el problema grande y se rompe en partes pequeñas. Es como si fuera un cambio de mentalidad, primero buscas de resuelver lo pequeño y luego construyes lo grande.

12. Defienda cuál de las tres versiones usaría si el programa debe responder 100,000 consultas de Fibonacci con distintos valores de n en una misma ejecución.

    En este caso, usaría la versión memorizada para las 100,000 consultas ya que conviene para así evitar recalcular tantos subproblemas. Esto se debe a que la versión recursiva ingenua sería extremadamente ineficiente para valores grandes ya que repite emucho el trabajo y lo hace más lenta. 

    De esta manera, una vez calculados los primeros Fib(n), cada consulta siguiente es casi instantánea (O(1)).


## Ejercicio 3 — Torres de Hanoi: cuando la explosión no es un defecto (30 puntos)
## Código base:
    public static void hanoi(int n, char origen, char auxiliar, char destino) {
        if (n == 1) {
            System.out.println(origen + " -> " + destino);
            return;
        }
        hanoi(n - 1, origen, destino, auxiliar);
        System.out.println(origen + " -> " + destino);
        hanoi(n - 1, auxiliar, origen, destino);
    }


13. Para n = 3, trace a mano la secuencia completa de llamadas y movimientos. Luego explique por qué el movimiento central del disco más grande divide la solución en dos problemas simétricos.

    1. Mover el disco 1 de A a C
    2. Mover el disco 2 de A a B
    3. Mover el disco 1 de C a B
    4. Mover el disco 3 de A a C
    5. Mover el disco 1 de B a A
    6. Mover el disco 2 de B a C
    7. Mover el disco 1 de A a C

    El movimiento central del disco grande divide el problema en dos pasos iguales: primero mover los discos pequeños de A a B, luego moverlos de B a C, pero teniendo el disco grande ya en C. Son dos subproblemas del mismo tipo, solo cambian los postes, por eso es simétrico.

14. Escriba la recurrencia M(n) del número mínimo de movimientos y resuélvala hasta llegar a la forma cerrada.

    Como se vio en la clase, se puede deducir su expresión de la siguiente manera:
    
    M(n) = 2 * M(n - 1) + 1 para n > 1
    M(1) = 1

    Resolviendo la recurrencia:
    
    M(2) = 2 * M(1) + 1 = 2 * 1 + 1 = 3
    M(3) = 2 * M(2) + 1 = 2 * 3 + 1 = 7
    M(4) = 2 * M(3) + 1 = 2 * 7 + 1 = 15
    M(5) = 2 * M(4) + 1 = 2 * 15 + 1 = 31

    Observando el patrón, podemos deducir que la forma cerrada de la recurrencia es: M(n) = 2^n - 1

15. Explique por qué Hanoi también tiene crecimiento exponencial, pero por una razón distinta a Fibonacci: aquí no hay recomputación inútil, sino que cada llamada representa trabajo estructuralmente necesario.

    En Hanoi, el crecimiento exponencial se debe a que cada movimiento del disco más grande requiere resolver dos subproblemas completos de tamaño n-1. No hay recomputación inútil porque cada llamada recursiva representa un paso necesario para mover los discos pequeños antes y después de mover el disco grande. En cambio, en Fibonacci, el crecimiento exponencial se debe a la recomputación de los mismos subproblemas múltiples veces, lo que no es estructuralmente necesario.

16. Argumente por qué no existe una versión iterativa trivial que mejore la complejidad del problema. Puede cambiar la forma de implementarlo, pero no el número mínimo de movimientos requerido.

    No hay forma iterativa “mágica” que lo haga más rápido, porque mover los discos mínimo ya es exponencial (2^n−1). Recursivo o iterativo, siempre hay que hacer esos movimientos, así que la complejidad sigue siendo O(2^n).


17. Diseñe una estrategia iterativa o con pila manual que simule la recursión. No se evalúa por ser más rápida, sino por demostrar que entendiste qué información guarda cada marco de llamada.

    Para simular la recursión de Hanoi sin usar la pila del sistema yo usaria la siguiente estrategia:

    - Guarda cada llamada como un estado en una pila.
    - Empuja el estado inicial (n, 'A', 'B', 'C').
    - Mientras la pila no esté vacía:
        - Saca el estado actual.
        Si n == 1, imprime origen -> destino y sigue.
        Si n > 1, apilaia en este orden:
        (n-1, auxiliar, origen, destino) (segundo subproblema)
        - Un estado de acción para imprimir origen -> destino (o imprime directamente después de desapilar)


18. Responda: ¿qué cambia si el objetivo del programa no es imprimir movimientos, sino únicamente contar cuántos se necesitan? Rediseñe la solución y compare sus costos.

    Si el objetivo es contar los movimientos en lugar de imprimirlos, la solución se puede rediseñar para simplemente calcular el número de movimientos sin necesidad de realizar las llamadas recursivas para cada movimiento. En este caso, se puede usar la fórmula cerrada M(n) = 2^n - 1 directamente, lo que reduce la complejidad a O(1) para calcular el número de movimientos necesarios.

    Comparando sus costos:
    - La versión original con impresión tiene una complejidad temporal de O(2^n) debido a la cantidad de movimientos que se deben imprimir.
    - La versión rediseñada para contar movimientos tiene una complejidad temporal de O(1) ya que solo se realiza un cálculo directo sin necesidad de iterar o hacer llamadas recursivas.

19. Por qué Hanoi es un excelente contraejemplo para el estudiante que cree que si una recursión es O(2^n), entonces está mal diseñada. Pista conceptual
En Fibonacci la pregunta clave es “qué se repite”. En Hanoi la pregunta clave es “qué es inevitable”. Si confundes esas dos ideas, tu análisis quedará incompleto.

    Hanoi es un excelente contraejemplo porque aunque tiene una complejidad de O(2^n), no está mal diseñada. Cada movimiento del disco más grande requiere resolver dos subproblemas completos de tamaño n-1. No hay recomputación inútil como en Fibonacci, sino que cada llamada representa trabajo estructuralmente necesario para resolver el problema.

## Ejercicio 4 — Mutaciones de código: cambiar la complejidad con cambios mínimos (25 puntos)
Analice cada fragmento. En todos los casos debe: identificar el costo dominante, proponer un cambio de diseño y justificar exactamente por qué cambia la complejidad.

## Fragmento A:

    public static int potencia(int x, int n) {
        if (n == 0) return 1;
            return x * potencia(x, n - 1);
    }

## Fragmento B:
    public static String invertir(String s) {
        if (s.length() <= 1) return s;
            return invertir(s.substring(1)) + s.charAt(0);
    }

## Fragmento C:
    public static int contarUnos(int[] a, int i) {
        if (i == a.length) return 0;
            return (a[i] == 1 ? 1 : 0) + contarUnos(a, i + 1);
    }


20. En el fragmento A, reemplace la idea recursiva lineal por exponenciación rápida. Debe justificar por qué el problema deja de reducirse de n en n y pasa a reducirse por mitades.

    La implementación sería algo como esto:

    public static int potencia(int x, int n) {
        if (n == 0) return 1;
        if (n % 2 == 0) {
            int power = potencia(x, n / 2);
            return power * power;
        } else {
            return x * potencia(x, n - 1);
        }
    }

21. En el fragmento B, explique por qué la lógica parece lineal, pero puede degradarse por el costo acumulado de concatenaciones y subcadenas. Luego proponga una versión que reduzca ese costo.

    public static String invertir(String s) {
        StringBuilder sb = new StringBuilder();
        for (int i = s.length() - 1; i >= 0; i--) {
            sb.append(s.charAt(i));
        }
        return sb.toString();
    }


22. En el fragmento C, argumente por qué cambiarlo a iterativo mejora el espacio pero no el tiempo asintótico. Luego responda: vale la pena cambiarlo siempre.

    Cambiarlo a iterativo mejora el espacio porque elimina la necesidad de mantener una pila de llamadas recursivas, lo que reduce la complejidad espacial de O(n) a O(1). Sin embargo, el tiempo asintótico sigue siendo O(n) en ambos casos, ya que se necesita recorrer todo el array para contar los unos.


    Mensajes finales: Se hizo rápido así que tomese un cafecito y sea suave ;-)
