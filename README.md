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

## Declaración de uso de IA

Herramientas utilizadas:
- **Gemini** (integrado en Google Colab): cada integrante lo usó en su propio Colab para generar código por partes.

Todo el código generado fue revisado y probado por el integrante responsable antes de subirlo.

| Parte | Integrante | Qué se solicitó | Qué se modificó | Cómo se verificó |
|---|---|---|---|---|
| 1. Terreno | Ivan Ortega | Matriz 10x10 con niveles de inundación, diccionario de costos, inicio, refugio y función para imprimir el mapa | Nada | Comprobé el shape (10, 10), la posición de I y R, y que la matriz coincide número por número con la definida por el equipo |
| 2.  |  |  |  |  |


## Historial de cambios

El desarrollo se registró con commits de Git desde Colab (Archivo → Guardar una copia en GitHub). Los mensajes de commit incluyen el nombre de quien realizó el cambio.
