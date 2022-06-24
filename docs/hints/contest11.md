---
title: contest 11 - hints y códigos de ejemplo
---

[Index](../index) > [Contests](../contests) > [Contest 11](../contests#contest-11) > ```{{page.title}}```

### A - Almost Shortest Path

<details>
  <summary>Hint 1</summary>
  Sea L(u,v) la distancia más corta desde u hasta v (si no existe un camino, L(u,v) = infinito). Una arista (u,v) es parte de algún camino más corto desde S a D si y sólo si L(S,u) + w_{u,v} + L(v,D) = L(S,D).
</details>
<details>
  <summary>Hint 2</summary>
  Notar que en el Hint 1 necesitamos ser capaces de calcular L(S,u) y L(u,D) para cualquier posible nodo u (recordar que S y D son fijos). Piensa en una forma de calcular eficientemente ambos para todos los nodos.
</details>
<details>
  <summary>Solución + código</summary>
  Para calcular L(S,u) para cada nodo u, corremos dijkstra desde S en el grafo G. Para calcular L(u,D), corremos dijkstra desde D sobre un grafo G' equivalente al grafo G con las aristas invertidas. Luego iteramos sobre todas las aristas (u,v) y aquellas que cumplan la propiedad del hint 1 las descartamos, y las demás las agregamos en nuevo grafo G''. Finalmente corremos un tercer dijkstra en G'' desde S y reportamos la distancia hasta D (o -1 si no se puede llegar). <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/SPOJ/SAMER08A_AlmostShortestPath.cpp">Código de ejemplo</a>
</details>

### B - Galactic Taxes

<details> 
  <summary>Hint</summary>
  Es posible demostrar que el "tax" asociado a la operación comercial se comporta como una función cóncava con respecto al tiempo. Para sacar el tax en un momento determinado basta con usar un algoritmo de shortest path básico como dijkstra sobre el grafo tomando los pesos en ese momento.
</details>
<details> 
  <summary>Solución + código</summary>
  La solución consiste en realizar ternary search sobre el tiempo para encontrar cuando se produce el máximo tax. En cada momento en la búsqueda se calcula el tax asociado con dijkstra con los pesos del momento.
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/URI/GalacticTaxes.cpp">Código de ejemplo</a>
</details>

### C - All Pairs Shortest Path

<details>
  <summary>Hint</summary>
  Por la materia vista, obviamente floyd warshall, pero cuidado con los casos bordes. Notar que el enunciado no menciona restricciones sobre sobre cómo puede ser el grafo. Eso quiere decir que en teoría podrían haber múltiples aristas entre dos nodos y también self-loops (de un nodo a sí mismo).
</details>
<details>
  <summary>Solución + código</summary>
  Básicamente floyd warshall con el extra para detectar ciclos negativos (ver materia en sección grafos) y teniendo cuidado con manejar los casos bordes mencionados. <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/kattis/AllPairsShortestPath.cpp">Código de ejemplo</a>
</details>

### E - Wormholes

<details>
  <summary>Hint</summary>
  Bellman Ford
</details>
<details>
  <summary>Solución + código</summary>
  Bellman Ford básicamente, más el extra para pillar ciclos negativos (ver materia sección grafos). <a href="https://github.com/PabloMessina/Competitive-Programming-Material/blob/master/Solved%20problems/UVA/558_Wormholes.cpp">Código de ejemplo</a>
</details>

### G - Keep It Energized
<details> 
  <summary>Hint 1</summary>
  Una forma de interpretar este problema como una de grafos constiste en tomar cada tienda como un nodo en un grafo implícito. En este grafo hay una arista entre una tienda A y otra B en un nivel superior si al comprar el paquete de energía de A tienes suficiente energía para llegar al nivel donde se encuentra B. De esta forma podemos interpretar un camino en el grafo como visitar las tiendas en las SI que compraremos paquetes energéticos.
</details>
<details> 
  <summary>Hint 2</summary>
  La respuesta final al problema consistirá en el camino más corto entre alguna tienda inicial y el final del juego (Lo podemos considerar como una tienda en el nivel N + 1 de costo 0).
</details>
<details> 
  <summary>Solución + código</summary>
  Para terminar basta con hacer uso de un algoritmo de camino más corto como dijkstra. Notar que no es necesario construir el grafo para resolver el problema. Sólo basta saber cuando hay una conexión entre tiendas y usar eso en la iteración de dijkstra. Podemos acelerar la búsqueda sólo considerando pasar a tiendas en niveles que no hemos llegado aún. Esto funciona pues como dijkstra ordena según costo para realizar la su iteración. No valdrá la pena pasar a un nivel al cual ya hemos ido (pues llegamos a el con un costo menor).
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/URI/KeepItEnergized.cpp">Código de ejemplo</a>
</details>

### H -  Roads In Berland
<details> 
  <summary>Hint</summary>
  Para este problema basta saber updatear la matriz de distancias mínimas dada la adición de una nueva arista. Para esto pueden considerar cómo funciona la iteración de Floyd Warshall (Pues considera caminos de todos a todos).
</details>
<details> 
  <summary>Solución + código</summary>
  Usando updates del estilo Floyd Warshall si se agrega una arista entre los nodos i y j de costo w podemos iterar cuadráticamente sobre cada par de nodos por ejemplo u y v y notar que la distancia entre ellos luego de un update será el mínimo entre tres valores. El costo previo a la adición de la nueva arista D[u][v], el costo de ir de u a i, pasar por la nueva arista e ir de j a v (D[u][i] + w + D[j][v]), y el orden contrario de la arista nueva (D[u][j] + w + D[i][v]). El mínimo entre estos tres valores será el nuevo D[u][v]. Si vamos tomando en cuenta estos cambios en una suma acumulada global podemos responder al problema. Este algoritmo será de complejidad cúbica (una pasada cuadrática por cada arista nueva).
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/RoadsInBerland.cpp">Código de ejemplo</a>
</details>

### I - Greg And Graph
<details> 
  <summary>Hint</summary>
  Podemos pensar el problema al revés. Es decir, imprimir la suma de costos luego de haber agregado un vértice y todas sus aristas a un grafo que inicialmente parte vacío. Para ir calculando y updateando los valores de distancias mínimas podemos usar un approach parecido al del problema anterior, sólo que el update cambia, ya que agregamos nodos, no aristas.
</details>
<details> 
  <summary>Solución + código</summary>
  Podemos mantener una matriz de costos mínimos entre nodos, para esto tener una lista de nodos activos (aquellos que ya han sido agregados), y cada vez que se agregue un nuevo nodo realizar tres cosas. Agregar a la matriz de costos mínimos desde y hacia el nuevo nodo (hacia y desde los nodos activos). Para esto si agregamos un nodo u, para calcualar su distancia mínima hacia un nodo v basta tomar el mínimo de la entre D[u][v] y D[u][w] + Dmin[w][v] con w en los nodos activos, la distacia hacia el otro lado se updatea de forma similar. Finalmente también pueden cambiar las distancias mínimas entre dos nodos que no sean el agregado u. Para updatear Dmin[v][w] basta con tomar el mínimo entre Dmin[v][w] y Dmin[v][u] + Dmin[u][w]. La complejidad final del algorítmo será cúbica.
  <a href="https://github.com/BenjaminRubio/CompetitiveProgramming/blob/master/Problems/Codeforces/GregAndGraph.cpp">Código de ejemplo</a>
</details>

<!-- <details> 
  <summary>Hint</summary>   
</details>
<details> 
  <summary>Solución + código</summary>
  <a href="">Código de ejemplo</a>
</details> -->

[Index](../index) > [Contests](../contests) > [Contest 11](../contests#contest-11) > ```{{page.title}}```
