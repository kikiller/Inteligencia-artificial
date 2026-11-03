# EXAMEN TEÓRICO — PERCEPTRÓN, MLP Y CNN (GRUPO A)

Eduardo Alcaraz

## 1 Datos del alumno

Nombre: Luis Enrique Martinez Rubio

Grupo: A

Fecha: 03/11/2026

## 2 Pregunta 1 — Idea del perceptrón

1. Explica qué recibe un perceptrón, qué hace con los pesos y el bias, y qué produce.
2. ¿Qué quiere decir que dos clases sean *linealmente separables*?
3. Señala *una* limitación fuerte del perceptrón de una sola capa.

### Respuesta

1. El perceptrón recibe un conjunto de entradas numéricas. Cada entrada se multiplica por su peso, todo se suma y a ese resultado se le añade el bias, que sirve para mover el punto en el que la neurona se activa. Esa suma pasa por una función de activación, normalmente un escalón, y el perceptrón produce una salida, por ejemplo 0 o 1, que es la clase que predice.

2. Que existe una recta, un plano o un hiperplano que deja todos los puntos de una clase de un lado y todos los de la otra clase del lado contrario, sin ningún error.

3. Solo resuelve problemas linealmente separables. Si las clases están acomodadas de forma que ninguna línea recta las divide, el perceptrón no logra aprenderlas. XOR es el ejemplo típico.

## 3 Pregunta 2 — Por qué aparece el MLP

1. ¿En qué sentido un MLP es “más de una capa de neuronas”?
2. ¿Para qué sirve la capa oculta ?
3. Si quitas todas las activaciones no lineales, ¿qué tipo de comportamiento te queda y por qué importa?

### Respuesta

1. Un MLP junta varias capas de neuronas una detrás de otra: una capa de entrada, una o más ocultas y una de salida. La salida de cada capa es la entrada de la siguiente, así que la información va pasando por varias transformaciones en vez de una sola.

2. La capa oculta crea representaciones intermedias. Combina las entradas originales en rasgos nuevos que hacen que el problema sea más fácil de separar para la capa de salida. Gracias a ella la red puede aprender relaciones que un perceptrón solo no puede.

3. Me queda un comportamiento lineal. Aunque haya muchas capas, varias transformaciones lineales seguidas equivalen a una sola transformación lineal. Esto importa porque entonces la red no tendría más poder que un modelo lineal simple y no podría resolver cosas como XOR ni aprender fronteras curvas. La no linealidad es lo que le da sentido a tener más capas.

## 4 Pregunta 3 — XOR como lección histórica

1. ¿Qué enseña el ejemplo XOR sobre el perceptrón simple?
2. ¿Qué cambia al pasar a una red con capas ocultas y no linealidad?
3. ¿Eso significa que el perceptrón nunca sirve en problemas reales? Argumenta.

### Respuesta

1. Enseña que un perceptrón simple no puede aprender una función cuyas clases no se separan con una recta. En XOR la salida es 1 cuando las dos entradas son distintas y 0 cuando son iguales, y no hay forma de trazar una sola línea que deje de un lado los unos y del otro los ceros.

2. Con una capa oculta y una activación no lineal, la red puede combinar varias rectas y formar fronteras más complejas. Con solo un par de neuronas ocultas ya se puede resolver XOR, porque cada una separa una parte del problema y la capa de salida junta los resultados.

3. No. El perceptrón sirve cuando el problema es más o menos lineal, o cuando quiero un modelo simple, rápido y fácil de interpretar. Muchos problemas reales con pocas variables se resuelven bien con modelos lineales, y además sirve como punto de comparación para saber si un modelo más grande de verdad mejora algo. También es la neurona base con la que se construyen las redes más grandes.

## 5 Pregunta 4 — Cómo “aprende” la red

1. Describe con palabras el ciclo: predicción → medir error → ajustar pesos.
2. ¿Qué controla el *learning rate* (tasa de aprendizaje) a nivel conceptual?
3. Diferencia *subajuste* y *sobreajuste* con un ejemplo escolar breve.

### Respuesta

1. La red recibe un dato y hace una predicción. Esa predicción se compara con la respuesta correcta usando una función de pérdida, que dice qué tan equivocada estuvo. Con ese error se calcula hacia qué lado hay que mover cada peso para equivocarse menos y se ajustan. Esto se repite con muchos ejemplos y durante varias épocas hasta que el error deja de bajar.

2. Controla qué tan grande es cada paso al ajustar los pesos. Si es muy alta, la red se pasa de largo y nunca se estabiliza. Si es muy baja, aprende lentísimo o se queda atorada en soluciones malas.

3. El subajuste sería estudiar solo el título de cada tema: repruebas tanto los ejercicios de práctica como el examen porque no aprendiste lo suficiente. El sobreajuste sería memorizar las respuestas de la guía: sacas 10 en la guía, pero cuando el examen cambia las preguntas repruebas, porque memorizaste en vez de entender.

## 6 Pregunta 5 — Salidas: sigmoide y softmax

1. ¿Cuándo usarías sigmoide y cuándo softmax?
2. ¿Qué significa el vector que sale de un softmax en un problema de varias clases?
3. Pon un ejemplo del curso (juego, riesgo, imágenes, etc.) y di qué salida encajaría.

### Respuesta

1. Usaría sigmoide cuando hay una sola salida que representa la probabilidad de una clase, como en una clasificación binaria, o cuando son varias etiquetas independientes entre sí. Usaría softmax cuando hay varias clases y solo una de ellas puede ser la correcta.

2. Es un vector de probabilidades con un valor por clase. Todos los valores están entre 0 y 1 y suman 1. La clase con el valor más alto es la predicción de la red, y el valor indica qué tan segura está de esa decisión.

3. Para saber si un alumno aprueba o no, la salida sería una sigmoide, porque solo hay una probabilidad de aprobar. Para clasificar el color de un semáforo en rojo, amarillo o verde a partir de una foto, encajaría softmax, porque son tres clases y solo una es la correcta.

## 7 Pregunta 6 — Motivación de la CNN

1. ¿Qué inconveniente conceptual tiene aplastar una imagen y meterla a un MLP denso?
2. ¿Qué aporta la idea de *filtro local* y *pesos compartidos*?
3. Menciona *dos* dominios (además de fotos cotidianas) donde una CNN tiene sentido.

### Respuesta

1. Al aplastar la imagen se pierde la estructura espacial, porque píxeles que estaban juntos quedan lejos en el vector y la red no sabe que eran vecinos. Además el número de parámetros se dispara, ya que cada neurona densa necesita un peso por cada píxel de entrada. Y el modelo se vuelve muy sensible a que el objeto se mueva un poco de lugar.

2. El filtro local mira solo una región pequeña de la imagen y detecta un patrón sencillo, como un borde. Los pesos compartidos significan que ese mismo filtro se desliza por toda la imagen, así que puede detectar el patrón en cualquier posición y se usan muchísimos menos parámetros que en una red densa.

3. Imágenes médicas, como radiografías o resonancias para detectar anomalías, e imágenes satelitales para identificar cultivos, zonas urbanas o cambios en el terreno.

## 8 Pregunta 7 — Piezas de una CNN

Explica el *para qué* de:
1. Convolución
2. Pooling
3. Flatten + Dense al final
4. Dropout durante el entrenamiento

### Respuesta

1. La convolución sirve para extraer rasgos locales de la imagen aplicando filtros que se deslizan por ella. Cada filtro genera un mapa de características que indica dónde aparece el patrón que busca.

2. El pooling reduce el tamaño de los mapas quedándose con lo más importante de cada zona. Así baja el costo de cómputo, se reducen los parámetros y la red tolera pequeños desplazamientos en la imagen.

3. Flatten convierte los mapas de características en un solo vector, y las capas Dense combinan todos esos rasgos para tomar la decisión final, por ejemplo qué clase es.

4. Dropout apaga al azar una parte de las neuronas en cada paso del entrenamiento. Esto evita que la red dependa demasiado de unas pocas neuronas y ayuda a reducir el sobreajuste. Solo se aplica al entrenar, no al evaluar ni al predecir.

## 9 Pregunta 8 — Capas bajas vs capas altas

1. ¿Qué suelen captar las primeras capas de una CNN frente a las más profundas?
2. Usa una analogía (lupas, detectores, “de simple a complejo”).
3. ¿Por qué “poner más capas” no garantiza mejor modelo?

### Respuesta

1. Las primeras capas captan cosas simples como bordes, líneas, esquinas, texturas y colores. Las capas más profundas combinan eso para reconocer partes más complejas y al final objetos completos, como una rueda, un ojo o un semáforo.

2. Es como ir armando un texto: primero se reconocen letras, luego palabras y al final oraciones con sentido. O como una fila de lupas, donde la primera solo ve trazos, la siguiente junta trazos para ver formas y la última junta formas para reconocer el objeto.

3. Porque más capas significan más parámetros, más costo de entrenamiento y más riesgo de sobreajuste, y además la red se puede volver más difícil de entrenar. Si los datos son pocos o malos, o la arquitectura no encaja con el problema, agregar capas no ayuda. Lo que importa es comprobarlo con datos de validación.

## 10 Pregunta 9 — Análisis: elige arquitectura

Elige *Perceptrón (o lineal simple)* / *MLP* / *CNN* y justifica en *una* frase:

1. Tabla de alumnos: promedio, faltas, horas de estudio → ¿aprueba?
2. Clasificar el color de un semáforo en una fotografía.
3. Clases en el plano que *no* se separan con una recta.
4. ¿Usarías CNN para autocompletar código o una secuencia larga de texto? ¿Sí/no y qué alternativa del curso mencionarías?

### Respuesta

1. Perceptrón o modelo lineal simple, porque son pocas variables numéricas en una tabla y el problema probablemente se separa de forma casi lineal.

2. CNN, porque es una imagen y el color del semáforo se detecta con patrones locales de píxeles que los filtros convolucionales captan bien.

3. MLP, porque su capa oculta con activaciones no lineales permite formar fronteras curvas que una sola recta no puede dar.

4. No, porque una CNN está pensada para datos con estructura de cuadrícula como las imágenes y no maneja bien el orden ni las dependencias largas de una secuencia. Usaría un modelo secuencial como una RNN, una LSTM o un Transformer.

## 11 Pregunta 10 — Análisis: mirada de proyecto

1. Train accuracy altísima y validation baja: ¿qué sospechas? ¿Qué dos preguntas le harías al equipo en la defensa?
2. ¿Por qué más neuronas o más capas no sustituyen un buen planteamiento del problema y buenos datos?
3. Escribe *tres* preguntas orales (tuyas) para verificar que distinguen perceptrón, MLP y CNN.

### Respuesta

1. Sospecho sobreajuste: el modelo memorizó los datos de entrenamiento y no generaliza. Le preguntaría al equipo cómo dividieron los datos y si pudo haber ejemplos repetidos entre entrenamiento y validación, y qué hicieron para controlar el sobreajuste, como dropout, parar el entrenamiento a tiempo o conseguir más datos.

2. Porque una red más grande solo aprende mejor lo que se le da. Si los datos son pocos, tienen sesgo o están mal etiquetados, o si el problema está mal definido o la métrica no corresponde con lo que se quiere lograr, la red aprende lo equivocado con más capacidad. Primero hay que definir bien el problema, las variables y la forma de medir, y después ajustar el modelo.

3. Primera: ¿qué tipo de problema no puede resolver un perceptrón y por qué un MLP sí? Segunda: ¿qué ventaja tiene una CNN sobre un MLP denso cuando la entrada es una imagen? Tercera: si tuvieran que predecir algo con los datos de una tabla con pocas columnas, ¿con qué modelo empezarían y por qué?
