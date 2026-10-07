\# Parte I. Conceptos y definiciones



\## Pregunta 1

\*\*¿Qué es un árbol de decisión y cuál es su objetivo principal dentro de un problema de clasificación?\*\*



Un árbol de decisión es un algoritmo de aprendizaje automático supervisado que utiliza una estructura de diagrama de flujo para tomar decisiones. Su objetivo principal en un problema de clasificación es dividir secuencialmente un conjunto de datos en subgrupos cada vez más puros basándose en los valores de sus características, hasta poder asignar una etiqueta de clase categórica a cada observación con la mayor certeza posible.



\---



\## Pregunta 2

\*\*Explique con sus propias palabras los siguientes elementos de un árbol de decisión:\*\*



\*   \*\*Nodo raíz:\*\* Es el nodo principal en la parte superior del árbol. Representa el conjunto de datos completo antes de cualquier división y evalúa la característica más importante que mejor separa los datos desde el principio.

\*   \*\*Nodo interno:\*\* Son los nodos intermedios que actúan como puntos de prueba para variables específicas. Reciben datos de un nodo superior y los dividen en nuevas ramas basándose en una condición o regla (por ejemplo, "¿la edad es mayor a 30?").

\*   \*\*Rama:\*\* Son las conexiones entre los nodos. Representan el resultado de la prueba realizada en un nodo (generalmente una respuesta del tipo "Verdadero" o "Falso") y dirigen el flujo de los datos hacia el siguiente nodo.

\*   \*\*Hoja (o nodo terminal):\*\* Son los nodos finales en la parte inferior del árbol que ya no se dividen. Representan la decisión final del modelo; es decir, la etiqueta de clase o categoría que se le asigna a los datos que lograron llegar hasta esa etapa.



\---



\## Pregunta 3

\*\*¿Qué es una red neuronal multicapa y qué función cumplen las siguientes capas?\*\*



Una red neuronal multicapa (también conocida como Perceptrón Multicapa) es un modelo computacional inspirado en el cerebro humano, compuesto por múltiples capas de neuronas artificiales interconectadas. Es capaz de modelar relaciones no lineales y complejas en los datos.



\*   \*\*Capa de entrada:\*\* Es la puerta de entrada de la red. Recibe los datos crudos (las características o variables independientes). El número de neuronas en esta capa corresponde al número de características de los datos. Esta capa no realiza cálculos matemáticos; solo transfiere la información a la siguiente capa.

\*   \*\*Capa oculta:\*\* Son las capas intermedias entre la entrada y la salida (puede haber una o muchas). Aquí ocurre el procesamiento principal. Las neuronas de estas capas aplican operaciones matemáticas (multiplicación por pesos, suma de sesgos y funciones de activación) para descubrir, extraer y aprender patrones y características ocultas de los datos.

\*   \*\*Capa de salida:\*\* Es la capa final que produce el resultado de la red neuronal. Dependiendo del problema, puede tener una sola neurona (para regresión o clasificación binaria) o múltiples neuronas (para clasificación multiclase), entregando una predicción final o la probabilidad de pertenecer a una clase.



\---



\## Pregunta 4

\*\*¿Qué representan los pesos y los sesgos dentro de una red neuronal? Explique también por qué sus valores cambian durante el entrenamiento.\*\*



\*   \*\*Pesos (Weights):\*\* Representan la fuerza o importancia de la conexión entre dos neuronas. Determinan cuánta influencia tiene la salida de una neurona sobre la neurona de la siguiente capa.

\*   \*\*Sesgos (Biases):\*\* Son valores constantes adicionales sumados a cada neurona (similares a la intersección \*b\* en la ecuación de una recta \*y = mx + b\*). Permiten que la función de activación se desplace, dando a la red flexibilidad para ajustarse mejor a los patrones de los datos, incluso si todas las entradas son cero.



\*\*¿Por qué cambian?\*\* 

Al inicio, los pesos y sesgos se inicializan con valores aleatorios. Durante el entrenamiento, la red realiza predicciones, mide qué tan equivocadas están (usando una función de pérdida o error) y utiliza un algoritmo (como la retropropagación y el descenso de gradiente) para ajustar estos valores iterativamente. Cambian con el único objetivo de minimizar el margen de error, "aprendiendo" así la mejor configuración matemática para predecir correctamente.



\---



\## Pregunta 5

\*\*¿Cuál es la principal diferencia entre la forma en que aprende un árbol de decisión y la forma en que aprende una red neuronal multicapa? Explique qué elementos aprende cada modelo.\*\*



La principal diferencia radica en el enfoque matemático y la estructura del aprendizaje:



\*   \*\*Forma de aprender:\*\* El \*\*árbol de decisión\*\* aprende de forma \*voraz\* (greedy) y jerárquica, buscando matemáticamente (usando métricas como la Ganancia de Información o el Índice Gini) cuál es el mejor punto de corte en una variable para dividir los datos paso a paso. Es un proceso de lógica condicional (if-then). Por su parte, la \*\*red neuronal\*\* aprende de forma \*diferencial y continua\*, ajustando ecuaciones matemáticas complejas a lo largo de muchas iteraciones (épocas) basándose en el cálculo de gradientes para reducir un error global.

\*   \*\*Qué aprenden:\*\*

&#x20;   \*   \*\*El árbol de decisión aprende:\*\* Reglas de partición o umbrales lógicos (ej. aprender que si el "Salario > $50,000" es un buen criterio para dividir a los clientes).

&#x20;   \*   \*\*La red neuronal aprende:\*\* Parámetros numéricos (los \*\*pesos\*\* y los \*\*sesgos\*\*) para las millones de conexiones entre sus neuronas.

```eof

