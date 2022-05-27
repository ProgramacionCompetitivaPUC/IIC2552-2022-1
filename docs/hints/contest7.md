---
title: contest 7 - hints y códigos de ejemplo
---

[Index](../index) > [Contests](../contests) > [Contest 7](../contests#contest-7) > ```{{page.title}}```

### A - Fabricating Sculptures
<details> 
  <summary>Hint 1</summary>
  Notar que una base válida se puede pensar como una secuencia de segmentos horizontales apilados, donde el primer segmento es de ancho S y cada siguiente segmento es de ancho menor o igual al anterior y está contenido dentro de los límites del segmento anterior, y la cantidad total de bloques usados es B. Entonces una forma fácil de modelar esto sería con un DP(s, b) = todas las bases válidas que se pueden armar, dado que el primer segmento es de ancho 's' y la cantidad de bloques total a usar es 'b'. La solución al problema original sería DP(S, B).
</details>
<details> 
  <summary>Hint 2</summary>
  El problema del DP(s, b) del hint 1 es que si lo implementamos ingenuamente, la recurrencia sería hacer una sumatoria sobre todos los posibles casos del siguiente segmento horizontal, es decir, DP(s, b) = sum k=1..min(s, b-s) {  (s-k+1) * DP(k, b-s) }. Esta solución es correcta, pero nos da TLE. ¿Por qué? Porque la tabla memo sería de 5000 x 5000 y llenar cada celda requiere hacer una sumatoria (un for loop de hasta 5000 pasos), o sea, 5000^3 = 1.25 x 10^11 (mega TLE). Hint: piensa en una forma de jugar algebráicamente con la sumatoria y luego aplica "DP dentro del DP" (inception) para que el costo de la sumatoria sea prácticamente constante y no lineal.
</details>
<details> 
  <summary>Solución + código</summary>
  Hacemos el DP propuesto en los hints 1 y 2. La sumatoria la podemos descomponer en dos términos: DP(s, b) = (s+1) * (sum k=1..min(s, b-s) { DP(k, b-s) }) -  (sum k=1..min(s, b-s) { k * DP(k, b-s) }). Cada una de esas sumatorias las podemos encapsular en una función que recibe como argumentos el techo del k y el índice de la columna sobre la que se suma, y las podemos memoizar. Al final es como tener 3 tablas memo de 5000 x 5000 (peor caso) y el costo total es llenar las 3 tablas, es decir O(B x S x 3). <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/Matcomgrader/FabricatingSculptures.cpp">Código de ejemplo</a>
</details>

### B - Cleaning Robot
<details> 
  <summary>Hint</summary> 
  Supongamos que calculamos la distancia más corta (menor número de movidas) para llegar desde el robot hacia cada celda, y desde cada celda sucia a cada otra celda. Luego podemos olvidarnos del poblema original y verlo como el problema del vendedor viajero (TSP): el robot es un viajero que quiere visitar cada celda sucia en el menor tiempo posible. TSP es un problema clásico de DP. Revisen los apuntes de DP, ahí hay material sobre TSP.
</details>
<details> 
  <summary>Solución + código</summary>
  Básicamente calculamos las distancias desde el robot a cada otra celda, y desde cada celda sucia a cada otra celda. Esto no es difícil de hacer, la intuición es que partimos desde una celda origen y cada celda adyacente tiene distancia 1, luego las adyacentes de las adyacentes (no visitadas) tienen 2 distancia, y así. Es decir, vamos visitando las celdas por capas, donde las celdas de la siguiente capa tienen distancia 1 más que las celdas de la capa anterior. Esto se puede hacer con Breadth First Search (BFS). Una vez que tenemos estas distancias calculadas, el problema se reduce a TSP (travelling salesman problem), un DP muy estándar con bitmask. <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/SPOJ/CLEANRBT_CleaningRobot.cpp">Código de ejemplo</a>
</details>

### C - Enigma
<details> 
  <summary>Hint</summary>
  Notemos que podemos pensar el problema en cuanto al resto en módulo N. El problema se reduce a intentar que el resto total sea 0 dado lós dígitos predefinidos y los dígitos por definir. Para saber que resto aporta un dígito en la posición x desde la derecha basta multiplicar el dígito por el resto de la potencia de 10 correspondiente y sacar módulo. Podemos tener estos restos de potencias de 10 precalculados para evitar mayor complejidad.
</details>
<details> 
  <summary>Solución + código</summary>
  Podemos construir un DP que dependa de un índice (i) y un resto (r) y responda a la pregunta de si es posible hacer que el número tomando desde el índice i en adelante genere resto r. Para esto basta iterar por los posibles valores en el dígito i y preguntar por el estado (i + 1, r') donde r' está modificado para considerar el resto aportado en el dígito recién definido. Si probamos los dígitos en orden del 0 al 9 nos aseguramos que la primera vez que se responda true estamos en la menor solución, guardamos el dígito probado y devolvemos true sin probar el resto de los dígitos. Finalmente la respuesta depende únicamente del estado (0, 0).
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/Enigma.cpp">Código de ejemplo</a>
</details>

### D - Gates of uncertainty
<details> 
  <summary>Hint</summary> 
  Notar que el output de correcto o equivocado de la salida en uno de los nodos depende únicamente del output y correctitud del mismo en los nodos de input. De esta forma podemos separar el problema en nodos. Por ejemplo si ambos nodos input tienen salidas correctas y el nodo en cuestión funciona bien, el output será correcto. Por otro lado si uno de los nodos inputs tiene un cero correcto yo soy capaz de generar un uno correcto (a menos de que el nodo esté fijo en 0).
</details>
<details> 
  <summary>Solución + código</summary>
  Podemos construir un DP que dependa de 3 cosas, nodo, output generado (a) y output correcto (b) y que nos cuente cuantas combinaciones del input (la parte que afecta a este nodo) generan a en el nodo cuando deberían generar b. Para esto podemos ocupar el hint y hacer un dp que sólo dependa de las combinaciones de output y output correcto de los nodos input del nodo. Hay 4 combinaciones para cada nodo de input por lo que el dp en un nodo depende de 16 combinaciones de sus nodos input, las que se separan en aportar a las distintas posibilidades de a y b (y si el nodo esta defectuoso). Traten de ver qué combinaciones de los inputs aportan a cada combinación de a y b en el nodo, y armar el DP a base de esa dependencia. La complejidad total de esta solucion es O(100000 * 2 * 2 * 16) donde 100000 * 2 * 2 sale de la cantidad de estados y 16 de la  máxima cantidad de subestados de los que depende un estado.
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/GatesOfUncertainty.cpp">Código de ejemplo</a>
</details>

### E - Wooden Fence
<details> 
  <summary>Hint</summary>
  Una forma de modelar el problema: tengo que contruir una cerca de largo horizontal L, poniendo tablas de izquierda a derecha, partiendo con una tabla de tipo i (1 <= i <= N) y orientación o (o = 0 (normal), o = 1 (rotado)). Luego de hacer eso, nos va a quedar como subproblema completar una cerca de largo L - W[i] (si o == 0) o bien L - H[i] (si o == 1), y donde la siguiente tabla a poner tiene que cumplir que la parte de abajo tiene que calzar con W[i] (si o == 0) o bien con H[i] (si o == 1). Entonces nos podemos poner en todos los casos de las posibles siguientes tablas y orientaciones de las mismas para seguir resolviendo el subproblema que nos queda, y es fácil darse cuenta que podemos explorar todo el universo de posibilidades con backtracking y contar. Pero eso daría TLE, así que lo memoizamos y nos queda un DP.
</details>
<details> 
  <summary>Solución + código</summary>
  Siguiendo la idea del hint, hacemos un DP(i, o, len) = la cantidad de formas distintas que podemos completar una cerca de largo 'len' (horizontal) poniendo tablas de izquierda a derecha, sujeto a que estamos obligados a partir poniendo primero una tabla de tipo 'i' con orientación 'o'. La recursión no es muy complicada, es básicamente iterar sobre todos los tipos de tablas (excluyendo 'i' ya que no podemos repetir el tipo consecutivamente), considerados las dos posibles rotaciones (mientras no sea cuadrada) y llamamos el DP recursivamente. <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/Codeforces/182E_WoodenFence.cpp">Código de ejemplo</a>
</details>

### F - Long Jumps
<details> 
  <summary>Hint</summary>  
  Piensen en usar DP.
</details>
<details> 
  <summary>Solución + código</summary>
  Podemos hacer un dp con estado el índice en que estamos y que responda la máxima suma partiendo de este índice, luego la suma será A[i] + dp(i + A[i]), basta luego tomar el máximo de dp(i) para todo i.
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/LongJumps.cpp">Código de ejemplo</a>
</details>

### G - Lucky Number Representation
<details> 
  <summary>Hint 1</summary> 
  Notemos que el problema es mucho más facil si el número es divisible por cuatro. En este caso se puede solucionar el problema dividiendo por cuatro y avanzando dígito por dígito. Si el dígito es menor o igual a seis, agregamos a esa cantidad de números un cuatro en el dígito correspondiente. Si es mayor a seis basta notar que siete cuatros es equivalente a agregar cuatro sietes por lo que agregando esto alcanzamos a rellenar lo equivalente a maximo nueve cuatros con cuatro sietes y dos cuatros, ocupando los seis espacios que tenemos. Se cumplirá que la suma de los números es igual al número original antes de ser dividido. Pero nuestro número no necesariamente es divisible por cuatro.
</details>
<details> 
  <summary>Hint 2</summary>
  Notemos además que podemos hacer un DP "naive" (ingenuo) que trate de elegir seis lucky numbers que sumen al número pedido, para esto podemos separar en estados que dependan del número a separar (N)  y en cuántos números separarlo (C), probar con todos los lucky numbers menores o iguales al número y cuando probamos el lucky number L el resultado depende del estado (N - L, C - 1). La respuesta estará dada por DP(N, 6), con N el número incial. El problema de este approach es que La complejidad es muy alta, sólo es viable para números hasta 10.000 por el tiempo pedido.
</details>
<details> 
  <summary>Solución + código</summary>
  Podemos usar ambos Hints y obtener una solución. Primero descomponemos el número original N en dos partes, N1 = (N % 4000) y N2 = N - N1, evidentemente podemos resolver la solución de N1 por el DP descrito en el Hint 2. Mientras que N1 es divisible por 4 por lo que se puede obtener su solución usando el Hint 1. Luego la respuesta será los números de ambas soluciones sumados correspondientemente. Sólo hay un caso borde en que N1 no tiene solución al ser muy pequeño, acá basta con tomar N1 = (N % 4000) + 4000 y N2 = N - N1.
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/LuckyNumberRepresentation.cpp">Código de ejemplo</a>
</details>

### H - Garland
<details> 
  <summary>Hint</summary>
  Como las transiciones dependen sólo de la paridad de los números podemos contar cuantos pares e impares faltan por poner y ver cómo ponerlos de manera inteligente.
</details>
<details> 
  <summary>Solución + código</summary>
  Podemos costruir un DP que calcule la mínima cantidad de transiciones para el garland desde el índice i en adelante cuando quedan X impares, Y impares y el ultimo número tenía paridad p. Luego la respuesta será el mínimo entre o poner un número impar en la posición i mas el estado (i + 1, X - 1, Y, 1) o poner un número par más el estado (i + 1, X, Y - 1, 0), tomando en cuenta las transiciones cuando la paridad que decidamos sea distinta a la del número anterior. La respuesta será la del estado (0, Xi, Yi, -1) donde Xi es la cantidad de impares restantes al inicio, Yi la de pares y -1 una paridad que no corresponde a impar ni a par (para no tener transiciones al principio).
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/Garland.cpp">Código de ejemplo</a>
</details>

<!-- <details> 
  <summary>Hint</summary>   
</details>
<details> 
  <summary>Solución + código</summary>
  <a href="">Código de ejemplo</a>
</details> -->

[Index](../index) > [Contests](../contests) > [Contest 7](../contests#contest-7) > ```{{page.title}}```
