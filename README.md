# ruta-inundacion-ia
# Ruta de evacuación en zona inundada

Prototipo de búsqueda ciega (BFS, DFS y UCS) para encontrar una ruta hacia un refugio considerando el riesgo de inundación del terreno. Actividad: "Cuando el camino más corto no es el mejor" (Fundamentos de IA).

## Integrantes y contribuciones

| Integrante | Contribución |
|---|---|
| Ivan Arturo Ortega Monroy |  |
|    | 
|    |
|    |
|    |

## Decisiones de diseño

- **Terreno:** matriz de 10x10. Niveles: `0` seco, `1` riesgo bajo, `2` riesgo medio, `3` riesgo alto, `-1` bloqueado (no transitable).
- **Costos de entrar a una celda:** 1 / 3 / 6 / 10 para los niveles 0 / 1 / 2 / 3. El costo se paga al entrar a la celda; la celda de inicio no cuesta.
- **Movimiento:** 4 direcciones (arriba, abajo, izquierda, derecha).
- **Estrategias de búsqueda:** BFS, DFS y UCS (costo uniforme).
  - BFS minimiza el número de pasos e ignora el riesgo.
  - DFS encuentra una ruta sin garantía de calidad.
  - UCS expande siempre el nodo con menor costo acumulado, por lo que es la estrategia que incorpora el riesgo en la decisión.
- **Cómo se decide la mejor ruta:** la de menor costo total (suma de costos de las celdas), no la de menos pasos.
- **Refugio inaccesible:** los algoritmos devuelven `None` y el sistema informa que no hay ruta.
- **Validación:** una función independiente verifica inicio, fin, adyacencia, celdas no bloqueadas y recalcula el costo desde cero.
- **Movimiento:** 4 direcciones, en orden fijo (arriba, derecha, abajo, izquierda) para que los resultados de BFS y DFS sean repetibles.
- **Costos:** seco = 1, riesgo bajo = 3, medio = 6, alto = 10. El costo se paga al entrar a una celda; la celda de inicio no cuesta.
- **Funciones puras:** las funciones reciben el mapa como parámetro (sin variables globales) para poder probar distintos escenarios con el mismo código.
- **Nodo expandido:** cada celda que se saca de la frontera para procesarla. Se cuenta igual en BFS, DFS y UCS para que la comparación sea justa.
- **Salida de los algoritmos:** (ruta, costo, nodos_expandidos). Si no hay ruta, (None, None, nodos_expandidos), para poder medir el trabajo realizado aun cuando el refugio es inaccesible.
- **BFS:** minimiza el número de pasos e ignora el riesgo; el costo de la ruta se calcula solo al final.
- **DFS:** versión iterativa con pila explícita (evita el límite de recursión). Marca como visitada la celda al sacarla de la pila y explora los vecinos en el mismo orden que BFS. No busca la mejor ruta, solo una ruta válida; su resultado depende del orden de exploración.
- **UCS:** expande siempre la celda con menor costo acumulado, usando heapq. Es aquí donde el riesgo entra en la decisión: el costo de entrar a cada celda sale del diccionario COSTOS, así que una ruta que pasa por zonas peligrosas acumula más costo y se explora después. Se marca la celda como expandida al sacarla de la cola (no al meterla) para garantizar que la ruta sea óptima, y las entradas obsoletas de la cola se descartan sin contarlas.
- **Ruta corta vs. ruta conveniente:** BFS minimiza pasos; UCS minimiza costo. En el mapa principal BFS da una ruta de 9 pasos con costo 32 y UCS da una de 21 pasos con costo 21.
- **Visualización:** semáforo de colores (verde, amarillo, naranja, rojo) para el riesgo creciente y negro para las zonas bloqueadas. Los valores de los títulos y de la leyenda salen del código, no se escriben a mano. La función de dibujo acepta rutas vacías para poder mostrar escenarios sin solución.
- **Validador independiente:** validar_ruta comprueba las condiciones del escenario con su propio código y no reutiliza obtener_vecinos ni costo_de_ruta; si lo hiciera, un error en esas funciones pasaría inadvertido. Revisa explícitamente los límites del mapa porque en NumPy los índices negativos no producen error. La función auxiliar refugio_es_alcanzable permite comprobar de forma independiente si un resultado "sin ruta" es correcto.
- **Cómo decide el sistema entre dos rutas al mismo refugio:** compara su costo total, que es la suma del costo de entrar a cada celda. Una ruta más corta pero que atraviesa zonas de riesgo alto acumula más costo que una más larga por zonas seguras, y UCS elige la de menor costo. En el mapa principal, BFS elige 9 pasos con costo 32 y UCS elige 21 pasos con costo 21. En el mapa de 3x5, BFS elige 4 pasos con costo 31 y UCS elige 8 pasos con costo 8.
- **El riesgo es lo que cambia la decisión:** en la prueba 5b se ejecuta UCS con todos los costos iguales a 1 y elige la ruta más corta. Al restaurar los costos reales, vuelve a elegir la ruta segura.
- **La mejor ruta depende del terreno:** en la prueba 4 se sube el riesgo del corredor sur y UCS cambia de ruta; su costo empata con el de BFS porque la alternativa segura dejó de serlo.
- **Marcar visitados al sacar de la cola (UCS):** es la forma correcta en general. En este escenario, como el costo de entrar a una celda solo depende de la celda de destino, marcar al meter daría el mismo resultado, por lo que no se usó como error intencional para la prueba extra.

## Declaración de uso de IA

Herramientas utilizadas:
- **Gemini** (integrado en Google Colab): cada integrante lo usó en su propio Colab para generar código por partes.

Todo el código generado fue revisado y probado por el integrante responsable antes de subirlo.

| Parte | Integrante | Qué se solicitó | Qué se modificó | Cómo se verificó |
|---|---|---|---|---|
| 1. Terreno | Ivan Ortega | Matriz 10x10 con niveles de inundación, diccionario de costos, inicio, refugio y función para imprimir el mapa | Nada | Comprobé el shape (10, 10), la posición de I y R, y que la matriz coincide número por número con la definida por el equipo |
| 2. Vecinos y costo de ruta | Ivan Ortega | Función obtener_vecinos (orden fijo: arriba, derecha, abajo, izquierda, sin bloqueados ni fuera de límites) y función costo_de_ruta (suma el costo de entrar a cada celda, sin contar el inicio), con pruebas assert | Nada | Pasaron los assert: vecinos en (2,0), (0,0) y (1,4); costo de la ruta directa = 32 (calculado a mano antes de ejecutar); ruta con celda bloqueada devuelve None. Revisé que las funciones reciben el mapa como parámetro y no usan globales |
| 3. BFS | Fernando Rivas | Función bfs con deque, que ignora el riesgo al decidir, con padres para reconstruir la ruta, contador de nodos expandidos y devolución de (ruta, costo, nodos_expandidos), con pruebas assert | Nada | Pasaron los assert: la ruta del mapa principal es la fila 2 completa (9 pasos, costo 32, calculado a mano antes de ejecutar); inicio = refugio; refugio rodeado de bloqueos devuelve None; inicio bloqueado devuelve None. Revisé que marca visitados al encolar y cuenta al desencolar |
| 4. DFS | Fernando Rivas | Función dfs iterativa con pila, que ignora el riesgo al decidir, con padres para reconstruir la ruta, contador de nodos expandidos y la misma salida que bfs, con pruebas assert | Nada | Pasaron los assert: la ruta empieza en el inicio, termina en el refugio, es adyacente paso a paso, sin celdas bloqueadas ni repetidas; el costo coincide con costo_de_ruta; no es más corta que la de BFS; inicio = refugio; refugio rodeado devuelve None; inicio bloqueado devuelve None. Revisé que usa pila (no recursión) y que mete los vecinos en orden inverso |
| 5. UCS | Ivan Ortega | Función ucs con heapq: cola de prioridad por costo acumulado, visitados marcados al sacar de la cola, descarte de entradas obsoletas, contador de nodos expandidos y la misma salida que bfs y dfs, con pruebas assert y comparativa BFS/DFS/UCS | Nada | Pasaron los assert: ruta válida (adyacente, sin bloqueos ni repetidas); costo 21 en el mapa principal (calculado a mano antes de ejecutar: baja por la columna 0, cruza por la fila 8 y sube por la columna 9, 21 celdas secas); costo menor o igual que BFS y DFS; inicio = refugio; refugio rodeado devuelve None; inicio bloqueado devuelve None. Revisé que usa heapq y marca visitados al sacar |
| 6. Visualización | Dante Rivas | Funciones dibujar_terreno y comparar_visual con matplotlib: mapa en colores por nivel de riesgo, ruta superpuesta, marcas de inicio y refugio, y comparación lado a lado con costo, pasos y nodos en el título (calculados desde las variables) | Nada | Comparé la imagen contra la tabla de resultados reales: los títulos coinciden (BFS 32/9/57, DFS 34/17/18, UCS 21/21/71); I y R en las posiciones correctas; la ruta de UCS evita las celdas rojas y cruza por la abertura de la fila 8. Confirmé que las funciones no fallan con ruta=None |
| 7. Validador | Jesús Sánchez | Función validar_ruta independiente (no reutiliza vecinos, costo ni algoritmos): comprueba inicio, refugio, adyacencia, límites del mapa, celdas bloqueadas y recalcula el costo desde cero. Función refugio_es_alcanzable con recorrido propio. Pruebas assert con rutas válidas e inválidas | No re realizó ningún ajuste | Pasaron los assert: las rutas de BFS, DFS y UCS son válidas con costos recalculados 32, 34 y 21; el validador rechaza rutas sin inicio, sin refugio, con saltos, con celdas bloqueadas, con costo reportado incorrecto, fuera de límites, None y vacías. Se revisó manualmente que el validador no llama a las funciones de los algoritmos |
| 8. Pruebas 1 a 3 | Victor Julián | **Prompt 8a:** función ejecutar_escenario (corre BFS, DFS y UCS, valida cada ruta contra el mapa del escenario y genera un DataFrame) y la prueba 1. **Prompt 8b:** pruebas 2a, 2b y 3 (bloqueo de una celda de la ruta en dos variantes y refugio inaccesible). El prompt original pedía todo junto, pero la primera respuesta de Gemini se cortó después de la prueba 1, por lo que lo dividí en dos | No se modificó como tal el segundo prompt, pero se agregó un promt más no previsto | Pasaron los assert y apareció el mensaje final. Valores calculados a mano antes de ejecutar: costo 32 de UCS al bloquear (8,5); 21 pasos de BFS al bloquear (2,5); 88 nodos expandidos en los tres algoritmos cuando el refugio queda aislado. Comprobé en las gráficas que las rutas cambian donde debían cambiar. Tras el corte, revisé el código de la celda para confirmar que faltaban las pruebas y comprobé que la segunda celda usa las funciones ya definidas, sin redefinirlas |
| 9a. Prueba 4 | Dante Rivas | Prueba de cambio de riesgo: se sube a nivel 3 el riesgo de dos celdas del corredor sur, se evalúa la ruta antigua en el mapa nuevo con el validador y se vuelve a ejecutar la búsqueda. Pruebas assert | Nada | Pasaron los assert. Valores calculados a mano antes de ejecutar: la ruta antigua de UCS pasa a costar 39 (21 + 9 + 9); la nueva ruta de UCS cuesta 32; BFS y DFS no cambian porque ignoran el riesgo. Comprobé en la gráfica que UCS dejó de cruzar por la fila 8 |
| 9b. Prueba 5 y 5b | Dante Rivas | Mapa distinto de 3x5 donde la ruta más corta atraviesa riesgo alto y la segura es más larga, y prueba de sensibilidad que pone todos los costos en 1 para comprobar que el riesgo es lo que cambia la decisión de UCS. Dividí los prompts de la Parte 9 en dos para evitar que la respuesta se cortara | Nada | Pasaron los assert. Valores calculados a mano antes de ejecutar: BFS costo 31 en 4 pasos; UCS costo 8 en 8 pasos; BFS expande 8 nodos; con todos los costos en 1, UCS elige la ruta corta con costo 4. Revisé en el código que COSTOS se modifica en su lugar y se restaura en un bloque finally, y que una comprobación final confirma que quedó con sus valores originales |
| 10a. Prueba extra | Jesús Sánchez | Verificador del costo óptimo con un método independiente (relajación repetida, sin heapq ni las funciones de vecinos), generador de mapas aleatorios con semilla fija y función verificar_mapa que comprueba, en 500 mapas, validez de rutas, ausencia de celdas repetidas, que UCS dé el costo óptimo y no cueste más que BFS o DFS, que BFS dé el mínimo de pasos, que "sin ruta" ocurra solo si el refugio es inalcanzable y que el mapa original no se modifique |  No se realizó ninguna modificación | Revisé en el código que el verificador no reutiliza las funciones de los algoritmos. Resultado de la ejecución: [N] mapas alcanzables, [N] no alcanzables, [N] con errores. Comprobé que se cubren ambos casos (con y sin ruta) |
| 10b. Prueba de la prueba | Jesús Sánchez | Un UCS defectuoso con un único error intencional (ordena la cola por el costo de la celda en vez del costo acumulado) para comprobar que el verificador lo detecta | No se realizó ninguna modificación | El verificador detectó el error en [N] de 500 mapas. Revisé que el defecto está en una sola línea y que el ucs real no se modificó. En la gráfica de un mapa donde falla, el UCS defectuoso da una ruta de mayor costo |

## Prueba adicional: ¿qué error buscaba detectar y por qué?

Buscaba detectar que UCS devolviera una ruta válida pero no óptima. Es el error más difícil de notar en este tipo de programas: la ruta existe, el validador la acepta y el costo reportado coincide con el recalculado, pero puede haber otra ruta más barata. Ninguna de las pruebas anteriores lo detectaría, porque todas comparan contra resultados calculados a mano para mapas concretos.

Por eso se comparó contra un verificador independiente del costo mínimo (sin heapq ni las funciones de vecinos) sobre 500 mapas aleatorios con semilla fija, incluyendo mapas sin ruta. Además se comprobó que la prueba tiene capacidad de detección: se introdujo a propósito un UCS defectuoso (ordena la cola por el costo de la celda en vez del costo acumulado) y el verificador lo detectó en [N] mapas.

Resultados reales: [N] mapas con ruta, [N] sin ruta, [N] errores en el UCS correcto.

## Incidencias durante el desarrollo

| Parte | Qué ocurrió | Cómo se detectó | Cómo se resolvió |
|---|---|---|---|
| 8 | Gemini entregó solo la prueba 1 y omitió las pruebas 2a, 2b y 3 y el mensaje final | La salida terminaba tras la prueba 1 y no aparecía "Parte 8: todas las pruebas pasaron"; al revisar el código, la celda terminaba ahí | Se dividió el prompt en dos y se pidió el resto en una celda nueva, indicando que no se redefinieran las funciones ya existentes |

## Historial de cambios

El desarrollo se registró con commits de Git desde Colab (Archivo → Guardar una copia en GitHub). Los mensajes de commit incluyen el nombre de quien realizó el cambio.

