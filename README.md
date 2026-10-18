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

## Declaración de uso de IA

Herramientas utilizadas:
- **Gemini** (integrado en Google Colab): cada integrante lo usó en su propio Colab para generar código por partes.

Todo el código generado fue revisado y probado por el integrante responsable antes de subirlo.

| Parte | Integrante | Qué se solicitó | Qué se modificó | Cómo se verificó |
|---|---|---|---|---|
| 1. Terreno | Ivan Ortega | Matriz 10x10 con niveles de inundación, diccionario de costos, inicio, refugio y función para imprimir el mapa | Nada | Comprobé el shape (10, 10), la posición de I y R, y que la matriz coincide número por número con la definida por el equipo |
| 2. Vecinos y costo de ruta | Ivan Ortega | Función obtener_vecinos (orden fijo: arriba, derecha, abajo, izquierda, sin bloqueados ni fuera de límites) y función costo_de_ruta (suma el costo de entrar a cada celda, sin contar el inicio), con pruebas assert | Nada | Pasaron los assert: vecinos en (2,0), (0,0) y (1,4); costo de la ruta directa = 32 (calculado a mano antes de ejecutar); ruta con celda bloqueada devuelve None. Revisé que las funciones reciben el mapa como parámetro y no usan globales |


## Historial de cambios

El desarrollo se registró con commits de Git desde Colab (Archivo → Guardar una copia en GitHub). Los mensajes de commit incluyen el nombre de quien realizó el cambio.
