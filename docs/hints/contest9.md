---
title: contest 9 - hints y códigos de ejemplo
---

[Index](../index) > [Contests](../contests) > [Contest 9](../contests#contest-9) > ```{{page.title}}```

### A - Merge Sort

<details> 
  <summary>Hint</summary>
  Necesitamos un arreglo que al llamar mergesort sobre él, se hagan un total de K llamadas. Pensar en una forma de simular la ejecución de mergesort distribuyendo hacia abajo las K llamadas totales que hay que hacer, y en vez de ordenar vamos poniendo valores desordenados (cosa de que al llamar el mergesort original se ejecuten esas mismas K llamadas que simulamos).
</details>
<details> 
  <summary>Solución + código</summary>
  Hacemos la misma recursión divide and conquer de mergesort(l, r, k), donde le agregamos un argumento extra k que nos dice cuantas llamadas tenemos que hacer. Si k == 1, entonces esta llamada en particular debe ser una llamada final (no más recursión hacia abajo), así que el subarreglo correspondiente debe estar ordenado (llenamos con valores crecientes). Si k > 1, entonces hay que decidir cómo repartir (k-1) llamadas entre las dos llamadas hijas. Pueden haber varias opciones. Una opción posible es tirar la mayor cantidad de llamadas a la izquierda y lo que sobre a la derecha. Como sea que distribuyamos, nos van a quedar los dos subarreglos hijos con valores asignados. Para garantizar que la unión de los dos subarreglos quede desordenada, le podemos sumar un offset a los valores del subarreglo hijo izquierdo para garantizar que todos esos valores sean mayores estrictos a los valores del subarreglo derecho (con eso queda sí o sí desordenado). Los casos bordes en que se retorna -1 son cuando el k supera el máximo de llamadas posibles o bien cuando k es par. <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/Codeforces/873D_MergeSort.cpp">Código de ejemplo</a>
</details>

### B - Painting Fence

<details> 
  <summary>Hint</summary>
  Si tienes una cerca de ancho N, piensa en las formas de pintar el rectángulo de ancho N y altura 1 ubicado en el piso (la base de la cerca). Lo puedes pintar con un brochazo horizontal (costo 1), pero luego te faltaría pintar todo lo de arriba (la misma cerca pero restándole 1 a todas las alturas), o bien puedes pintar el rectángulo con N brochazos verticales (costo N, pero con eso pintas la cerca completa). Mezclar brochazos horizontales y verticales para el rectángulo basal no tiene sentido ya que en ese caso aprovechas de pintar el rectángulo entero con un puro brochazo horizontal y te ahorras todos los brochazos verticales. Ahora, no es dificil generalizar el razonamiento a todo el rectángulo basal de altura hmin, donde hmin es la altura mínima de la cerca.
</details>
<details> 
  <summary>Solución + código</summary>
  Hacemos una función recursiva para pintar paint(l, r, h) que calcula el costo óptmo de pintar la subcerca entre los índices l y r y considerando todo lo que está arriba de la altura h. El problema original se resuelve con paint(0, N, 0). Entonces en cada llamada tenemos dos opciones, pintar el rectángulo que va desde h hasta hmin(l, r) con brochazos horizontales (con lo cual nos quedarían subsubcercas aisladas por pintar recursivamente) o bien pintamos todo vertical de un viaje. Retornamos el mínimo entre ambas opciones. <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/Codeforces/448C_PaintingFence_v2.cpp">Código de ejemplo</a>
</details>

### D - Practice

<details> 
  <summary>Hint</summary>
  Notar que si tenemos un grupo de n personas y queremos repartirlos en dos grupos que maximicen la cantidad de pares, lo óptimo es repartidos en dos grupos de n/2 (si n es par) o lo más cercano a eso (floor(n/2) y n-floor(n/2)).
</details>
<details> 
  <summary>Solución + código</summary>
  Hacemos una función search(l, r, i) que reparte los jugadores l, l+1, l+2, ..., r-1 entre dos equipos desde la sesión de práctica i en adelante (la profundida de la recursión corresponde al índice de la sesión de práctica). En cada llamada, calculamos m = (l+r)/2, entonces los jugadores desde l hasta m-1 se van al equipo 1 en la sesión de práctica i. Luego se llama a search(l, m, i+1) y search(m, r, i+1). <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/Codeforces/234G_Practice.cpp">Código de ejemplo</a>
</details>

### E - Code For 1

<details> 
  <summary>Hint</summary>
  Pensar que tenemos un árbol binario donde la raíz es n, las dos nodos hijos inmediatos son floor(n/2) y floor(n/2), luego en el tercer nivel hay 4 nodos floor(floor(n/2)/2), etc. Cada nodo está a cargo de un subrango de índices del arreglo final. Por ej. la raíz n genera la lista completa, así que su rango es todo el arreglo, o sea [0, size(n)-1], donde size(n) es el tamaño del arreglo final generado por n. Pensar en una forma de responder la consulta [l, r] como si estuvieramos navegando este árbol binario implícito, y descartamos nodos que no aportan a la consulta (por ej. si un nodo está a cargo del rango [i, j] y dicho rango tiene intersección vacía con [l, r], podemos descartar ese nodo y todo su subárbol para abajo).
</details>
<details> 
  <summary>Solución + código</summary>
  Hacemos un divide and conquer navegando recursivamente sobre el árbol binario implícito explicado en el hint, donde en cada llamada recursiva vamos pasando hacia abajo el rango de índices correspondiente a cada nodo, y descartamos nodos que no aportan a la query [l,r]. Si un nodo está completamente contenido en la query, podemos retornar altiro la cantidad de 1s que hay en ese nodo (no es necesario seguir haciendo recursión). <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/Codeforces/768B_CodeFor1.cpp">Código de ejemplo</a>
</details>

### F - Tricky Function
<details> 
  <summary>Hint</summary>
  Notemos que si precalculamos las sumas parciales en un arreglo s, donde s[i] representa la suma de a[1] hasta a[i] entonces podemos reescribir f(i, j) = (j - i)^2 + (s[j] - s[i])^2. Notemos que minimizar esto es lo mismo que encontrar la distancia mínima entre puntos del estilo (i, s[i]) en el plano 2D. Luego el problema se reduce a encontrar el par de puntos más cercanos en un set.
</details>
<details> 
  <summary>Solución + código</summary>
  Una forma naive para encontrar el par de puntos más cercanos es checkear cada par, lo que es O(n^2) que no pasa en tiempo.
  Hay un approach clásico divide and conquer para solucionar el problema de par de puntos más cercanos en O(n*log(n)). Para aprender más al respecto pueden revisar los siguientes links: <a href="https://www.geeksforgeeks.org/closest-pair-of-points-using-divide-and-conquer-algorithm/">link1</a>, <a href="https://www.geeksforgeeks.org/closest-pair-of-points-onlogn-implementation/?ref=rp">link2</a>.
  
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/TrickyFunction.cpp">Código de ejemplo</a>
</details>

### G - Money for Nothing
<details> 
  <summary>Hint 1</summary>
  Notemos que podemos reducir el problema a ver los pares de (fecha, precio) como puntos en el plano y se busca un par de puntos en el plano tal que el rectángulo con esquina inferior en el set de puntos de venta y esquina superior en el set de puntos de compra sea el de mayor área.
</details>
<details> 
  <summary>Hint 2</summary>
  Siguendo lo anterior podemos notar que hay algunos puntos que podemos descartar, pues nunca serán parte del óptimo, por ejemplo cualquier punto (x, y) en el grupo de venta tal que existe (x', y') en el mismo grupo con x'<=x, y y'<= y. O cualquier punto (x, y) en el grupo de compra tal que existe (x', y') en el mismo grupo con x<=x' y y<=y'. Luego de haber descartado esta clase de puntos tendremos que si ordenamos los puntos de cada grupo respecto a su eje x, obtendremos puntos que aumentan en x y disminuyen en y.
</details>
<details> 
  <summary>Solución + código</summary>
  Finalmente usando todo lo anterior podemos hacer un análisis similar al realizado para el problema E, donde si el óptimo punto en el grupo de compra para un punto específico en el grupo de venta es en el índice i, entonces para un punto mayor en el grupo de venta el óptimo se debe alcanzar de i a la derecha. Usando una recurrencia similar a E pero maximizando en vez de minimizar, se obtiene la solución.
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Kattis/MoneyForNothing.cpp">Código de ejemplo</a>
</details>

### H - Boat Burglary
<details> 
  <summary>Hint</summary>
  Son 30 objetos a lo más, poquitos. Es tentador usar fuerza bruta. El problema es que iterar sobre todos los subconjuntos tomaría 2^30 = 1024^3 > 10^9, y eso daría el medio TLE. Pero recordemos que este contest es de divide and conquer. ¿Qué pasa si repartimos los 30 objetos en dos grupos de 15 y 15, y calculamos todos los subconjuntos para cada mitad? Esto tomaría 2 x 2^15 = 65536, lo cual es poquísimo. Piensa en una forma de resolver el problema original combinando de alguna forma estas dos mitades.
</details>
<details> 
  <summary>Solución + código</summary>
  Separamos los N objetos en dos mitades iguales (o casi iguales si N es impar). Por cada mitad iteramos sobre todos los posibles subconjuntos de elementos y guardamos en una lista/arreglo la suma de los pesos y la cantidad de objetos (una lista de pares). Ordenamos la segunda lista de menor a mayor lexicográficamente e iteramos sobre la primera lista. Para cada par de la primera lista, hacemos dos binary searches para encontrar todos los pares de la segunda lista cuya suma de pesos completa lo que nos falta para llegar a un peso total de D (el primer binary search es un lowerbound y el segundo un upperbound). Si el primero y el último par de ese rango tienen cantidades de objetos distintas, estamos en un caso ambiguo. Otro caso ambiguo es que encontremos respuestas válidas más de una vez y la cantidad de objetos sea distinta. Si nunca encontramos un caso válido, es imposible. Si logramos encontrar al menos un caso válido y todos los casos válidos siempre dieron la misma cantidad de objetos, esa es la respuesta. <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/SPOJ/BURGLARY_BoatBurglary.cpp">Código de ejemplo</a>
</details>

<!-- <details> 
  <summary>Hint</summary>   
</details>
<details> 
  <summary>Solución + código</summary>
  <a href="">Código de ejemplo</a>
</details> -->

[Index](../index) > [Contests](../contests) > [Contest 9](../contests#contest-9) > ```{{page.title}}```
