# 10 PREGUNTAS REDES NEURONALES CONVOLUCIONALES (CNN)

Eduardo Alcaraz

## Índice

1. [Instrucciones (alumnos)](#1-instrucciones-alumnos)
2. [Pregunta 1 — Convolución a mano (3×3)](#2-pregunta-1--convolución-a-mano-33)
3. [Pregunta 2 — Contar pesos](#3-pregunta-2--contar-pesos)
4. [Pregunta 3 — ¿Qué cambia si meto más capas?](#4-pregunta-3--qué-cambia-si-meto-más-capas)
5. [Pregunta 4 — Formas: de volumen a vector](#5-pregunta-4--formas-de-volumen-a-vector)
6. [Pregunta 5 — Softmax y el error “casi seguro”](#6-pregunta-5--softmax-y-el-error-casi-seguro)
7. [Pregunta 6 — Padding, stride y “se me achicó la imagen”](#7-pregunta-6--padding-stride-y-se-me-achicó-la-imagen)
8. [Pregunta 7 — Dropout: ¿dónde y para qué?](#8-pregunta-7--dropout-dónde-y-para-qué)
9. [Pregunta 8 — Diseño: 5 clases de animales](#9-pregunta-8--diseño-5-clases-de-animales)
10. [Pregunta 9 — CNN vs MLP vs RNN (mismo problema, mala herramienta)](#10-pregunta-9--cnn-vs-mlp-vs-rnn-mismo-problema-mala-herramienta)
11. [Pregunta 10 — Experimento mental: mover un gato](#11-pregunta-10--experimento-mental-mover-un-gato)
12. [Autoevaluación rápida (opcional)](#12-autoevaluación-rápida-opcional)
13. [Notas al pie (fórmulas útiles — no sustituyen el razonamiento)](#13-notas-al-pie-fórmulas-útiles--no-sustituyen-el-razonamiento)

---

## 1 Instrucciones (alumnos)

- Responde con lápiz y papel (o en este mismo buffer de Org).
- No basta con definir de memoria: hay que calcular, comparar o justificar.

---

## 2 Pregunta 1 — Convolución a mano (3×3)

Tienes este trozo de imagen en escala de grises (un canal) y este kernel:

```
Imagen 5×5                 Kernel 3×3
0  0  1  0  0              1  0 -1
0  1  2  1  0              1  0 -1
1  2  3  2  1              1  0 -1
0  1  2  1  0
0  0  1  0  0
```

### Tareas

1. Con stride = 1 y padding valid (sin relleno), ¿cuál es el tamaño del mapa de salida?
2. Calcula solo el valor de la esquina superior izquierda del mapa (la primera posición del filtro). Muestra la suma de productos.
3. ¿Este kernel parece detectar bordes verticales u horizontales? Justifica mirando los signos.

### Respuestas

1.- Con la fórmula floor((N - K)/S) + 1 = floor((5 - 3)/1) + 1 = 3 en cada eje. Salida: 3×3 (9 valores).

2.- La primera posición del filtro cubre las filas 0–2 y las columnas 0–2 de la imagen:

```
0  0  1        1  0 -1
0  1  2   ⊙    1  0 -1
1  2  3        1  0 -1
```

Producto elemento a elemento, fila por fila:

- Fila 1: 0·1 + 0·0 + 1·(-1) = -1
- Fila 2: 0·1 + 1·0 + 2·(-1) = -2
- Fila 3: 1·1 + 2·0 + 3·(-1) = 1 - 3 = -2

Suma total: -1 + (-2) + (-2) = -5

3.- Detecta bordes verticales. Cada fila del kernel es [1, 0, -1]: la columna izquierda pesa +1, la central 0 y la derecha -1. El kernel calcula, en esencia, "intensidad a la izquierda − intensidad a la derecha". La respuesta es grande (en valor absoluto) cuando hay un cambio brusco de izquierda a derecha, es decir, una frontera vertical, y es cercana a 0 en zonas uniformes o en cambios solo de arriba a abajo (las tres filas son idénticas, así que no distinguen cambios verticales de intensidad). Es el filtro tipo Prewitt/Sobel en x.

---

## 3 Pregunta 2 — Contar pesos

Una imagen RGB de 28×28×3. Compara dos modelos:

- Modelo A (MLP ingenuo): aplastas la imagen a un vector y conectas todas las entradas a una capa densa de 64 neuronas (sin bias por ahora).
- Modelo B (CNN): una Conv2D con 16 filtros de 3×3, padding='same', sobre la misma imagen (con bias: un bias por filtro).

### Tareas

1. ¿Cuántos pesos tiene la capa densa del Modelo A? (fórmula y número)
2. ¿Cuántos parámetros tiene la Conv2D del Modelo B? (pesos de kernels + biases)
3. En una frase: ¿por qué la CNN suele generalizar mejor con el mismo número de imágenes de entrenamiento?

### Respuestas

1.- Entradas: 28 · 28 · 3 = 2352. Con la fórmula n_in · n_out = 2352 · 64 = 150,528 pesos.

2.- Con la fórmula Kh · Kw · Cin · Cout + Cout = 3 · 3 · 3 · 16 + 16 = 432 + 16 = 448 parámetros. (El padding same no cambia el conteo de parámetros; solo el tamaño de la salida.)

3.- La CNN tiene muchísimos menos parámetros (448 vs. 150,528) y los reutiliza en toda la imagen (pesos compartidos y conexiones locales), de modo que aprende patrones útiles en cualquier posición con pocos datos y tiene menos capacidad para memorizar el conjunto de entrenamiento.

---

## 4 Pregunta 3 — ¿Qué cambia si meto más capas?

Parte del notebook CNNriesgo (simplificado):

```
Entrada (21×28×3)
  → Conv2D(32, 3×3, same) + LeakyReLU
  → MaxPool(2×2)
  → Flatten → Dense(32) → Dense(nClasses, softmax)
```

Quieres apilar un segundo bloque convolucional antes del Flatten:

```
… → Conv2D(32) → Pool → Conv2D(64, 3×3, same) → Pool → Flatten → …
```

### Tareas

1. Tras el primer MaxPool(2×2) con padding same sobre 21×28, ¿qué tamaño aproximado (alto×ancho) esperas? (puedes redondear / usar la regla “casi la mitad”)
2. ¿Qué significan ahora los 64 filtros de la segunda Conv2D: detectan bordes “crudos” o patrones más compuestos? Explica con la analogía de la lupa / capas abstractas.
3. Riesgo: si añades muchas capas Conv+Pool con pocas imágenes, ¿qué falla típico verías en las curvas train vs validation? ¿Qué harías primero para mitigarlo (Dropout, más datos, menos filtros…)? Elige una medida y justifica.

### Respuestas

1.- Con padding same, la salida es ceil(N/2): ceil(21/2) = 11 y 28/2 = 14. Aproximadamente 11×14 (con 32 canales: 11×14×32).

2.- Ya no miran píxeles crudos, sino los mapas de características de la primera capa. Los 32 filtros iniciales detectan cosas simples (bordes, esquinas, cambios de color). Cada filtro de la segunda capa combina esos detectores en una ventana 3×3, y gracias al pooling ese 3×3 cubre una región mayor de la imagen original (campo receptivo más grande). Por eso detectan patrones más compuestos: texturas, curvas, esquinas con cierta orientación, partes de objetos. Es como ir acercando una lupa: la primera lente ve trazos finos; la segunda mira cómo se ensamblan esos trazos en formas, y así cada capa es más abstracta que la anterior.

3.- Falla típica: sobreajuste (overfitting). La pérdida/accuracy de train mejora de forma constante (accuracy cercana a 100 %, loss bajando), mientras que la de validation se estanca o empeora (la val loss sube después de cierta época), con una brecha grande entre ambas curvas.

Medida elegida: aumentar los datos con data augmentation (o conseguir más imágenes). El origen del problema es que hay pocas imágenes para tanta capacidad del modelo; ofrecer más variedad (giros, desplazamientos, brillo, zoom) ataca directamente la causa, no solo el síntoma, y no reduce la capacidad de aprender patrones útiles como sí lo haría quitar filtros. Dropout o menos filtros también ayudan, pero son segunda opción.

---

## 5 Pregunta 4 — Formas: de volumen a vector

Supón que, justo antes del Flatten, el tensor tiene forma (7, 7, 64) (lote omitido: una sola imagen).

### Tareas

1. ¿Cuántos números salen del Flatten? (un solo entero)
2. Si la siguiente Dense tiene 32 neuronas (con bias), ¿cuántos parámetros tiene esa Dense?
3. Si en vez de Flatten usas Global Average Pooling (un promedio por canal), ¿qué forma tendrías a la salida y cuántos valores serían? ¿Ventaja frente a Flatten cuando la imagen de entrada es más grande?

### Respuestas

1.- 7 · 7 · 64 = 3136

2.- Con la fórmula n_in · n_out + n_out = 3136 · 32 + 32 = 100,352 + 32 = 100,384 parámetros.

3.- Se promedia cada mapa 7×7 y queda un valor por canal: forma (64,), es decir, 64 valores.

Ventaja: el tamaño de salida depende solo del número de canales, no del tamaño espacial de la imagen. Con Flatten, si la imagen de entrada crece, el vector crece (más alto×ancho), y con él los parámetros de la Dense siguiente (se dispara el conteo y el riesgo de sobreajuste). Con GAP el vector sigue siendo de 64, la Dense(32) tendría solo 64 · 32 + 32 = 2080 parámetros y el modelo puede aceptar imágenes de tamaños distintos.

---

## 6 Pregunta 5 — Softmax y el error “casi seguro”

Una CNN de 3 clases (inundación, incendio, tornado) saca logits que, tras softmax, dan:

```
P = [0.10, 0.05, 0.85]
```

La etiqueta verdadera (one-hot) es [0, 1, 0] (incendio).

### Tareas

1. ¿Qué clase predice el modelo (argmax)?
2. ¿Está “seguro” o “confundido”? Usa los números, no adjetivos vacíos.
3. Sin calcular la fórmula exacta: argumenta si la categorical crossentropy de este ejemplo será alta o baja, y por qué (pista: castiga cuando la probabilidad de la clase correcta es pequeña).

### Respuestas

1.- El máximo está en la posición 3 (0.85): predice tornado.

2.- Está seguro, pero equivocado. Asigna 0.85 a una sola clase, que es mucho más que las otras dos (8.5 veces más que la de inundación y 17 veces más que la de incendio), así que no hay duda entre candidatos: no es una distribución repartida. Sin embargo, a la clase correcta (incendio) le da apenas 0.05, es decir, 5 %. Es un error con alta confianza.

3.- Será alta. Con etiqueta one-hot, la pérdida solo mira la probabilidad asignada a la clase correcta: -log(p_correcta). Como p = 0.05 es muy pequeña, el logaritmo es muy negativo y la pérdida resulta grande (del orden de 3, frente a ~0 si hubiera puesto casi toda la probabilidad en incendio). La crossentropy castiga especialmente los errores cometidos con mucha confianza.

---

## 7 Pregunta 6 — Padding, stride y “se me achicó la imagen”

Entrada 32×32, kernel 5×5, un filtro, un canal.

### Tareas

Completa la tabla (escribe el alto×ancho de salida):

| padding | stride | salida (alto×ancho) |
|---------|--------|---------------------|
| valid   | 1      |                     |
| valid   | 2      |                     |
| same    | 1      |                     |

Luego: si encadenas tres MaxPool(2×2) seguidos sobre una imagen 32×32 (sin cuidar bordes), ¿qué problema práctico aparece para clasificar objetos pequeños o detalles finos?

### Respuestas

| padding | stride | salida (alto×ancho) |
|---------|--------|---------------------|
| valid   | 1      | 28×28               |
| valid   | 2      | 14×14               |
| same    | 1      | 32×32               |

- valid, stride 1: floor((32 - 5)/1) + 1 = 27 + 1 = 28, es decir 28×28
- valid, stride 2: floor((32 - 5)/2) + 1 = floor(13.5) + 1 = 13 + 1 = 14, es decir 14×14
- same, stride 1: la salida conserva el tamaño, es decir 32×32

Con tres MaxPool(2×2) seguidos: 32 → 16 → 8 → 4, la imagen queda en 4×4. Cada celda final resume un bloque de 8×8 píxeles originales, así que un objeto pequeño o un detalle fino (de unos pocos píxeles) se pierde o se diluye al quedarse con solo el máximo de cada bloque; se pierde resolución espacial y la información fina de localización y textura ya no llega a las capas finales. Para detalles finos conviene menos pooling, strides más suaves o mayor resolución de entrada.

---

## 8 Pregunta 7 — Dropout: ¿dónde y para qué?

En CNNriesgo hay Dropout(0.5) después del pooling y otra vez antes de la salida.

### Tareas

1. Durante entrenamiento, ¿qué hace Dropout(0.5) en una capa densa de 32 neuronas (en promedio cuántas “sobreviven” por paso)?
2. Durante inferencia (predict), ¿se apagan neuronas al azar? ¿Por qué sí o no?
3. Un compañero pone Dropout solo en la última época “para que no se sobreajuste al final”. ¿Tiene sentido? Critica la idea en 3–5 líneas.

### Respuestas

1.- En cada paso apaga (pone en cero) cada neurona con probabilidad 0.5, de forma aleatoria e independiente. En promedio sobreviven 16 de las 32 neuronas, y las que sobreviven se reescalan (×2) para mantener la magnitud esperada de la activación. Cada paso entrena, en la práctica, una subred distinta.

2.- No. En predict Dropout está desactivado: se usan todas las neuronas. Se busca una salida determinista y estable (la misma imagen debe dar siempre la misma predicción), y aprovechar toda la capacidad aprendida. Como en entrenamiento se reescaló por 2 (inverted dropout), no hace falta ajustar nada en inferencia: las activaciones ya tienen la escala correcta.

3.- No tiene sentido. Dropout es un regularizador que actúa durante todo el entrenamiento, evitando que las neuronas se co-adapten y que la red memorice; el sobreajuste se acumula época tras época, así que si se activa solo al final llega tarde: los pesos ya están sobreajustados y una sola época con ruido no lo deshace. Además, meter ruido aleatorio justo al final perturba la convergencia cuando se busca estabilizar el modelo. La forma correcta es usarlo desde el inicio (y vigilar la brecha train/val para ajustar la tasa), no como parche final.

---

## 9 Pregunta 8 — Diseño: 5 clases de animales

Debes adaptar CNNriesgo a: ranas, arañas, monos, ballenas, pájaros.

### Tareas

1. ¿Cuántas neuronas debe tener la última Dense y qué activación? ¿Por qué?
2. Si todas las fotos entran a 224×224×3 pero el modelo tiene input_shape=(21,28,3), ¿qué debe pasar antes del fit? (una frase operativa)
3. Propón una mejora de arquitectura (segundo Conv, BatchNorm, data augmentation…) y di qué síntoma en train/val te haría pensar que esa mejora era necesaria.

### Respuestas

1.- 5 neuronas con activación softmax. Una neurona por clase (rana, araña, mono, ballena, pájaro), y softmax convierte los logits en una distribución de probabilidad que suma 1, adecuada para clasificación multiclase de etiqueta única (cada foto es un solo animal). Se entrena con categorical_crossentropy (o sparse_categorical_crossentropy si las etiquetas son enteros).

2.- Hay que redimensionar todas las imágenes a 21×28×3 (y normalizar los píxeles, p. ej. dividir entre 255) antes de fit; la alternativa es cambiar el input_shape del modelo a (224,224,3).

3.- Mejora propuesta: data augmentation (giros horizontales, pequeñas rotaciones, zoom, desplazamientos, cambios de brillo). Síntoma que la justifica: la accuracy de train sube mucho (cercana a 100 %) mientras la de validation se estanca en un valor bajo, y la val loss empieza a subir mientras la train loss sigue bajando: sobreajuste por pocos datos. Al ampliar artificialmente la variedad de ejemplos, el modelo ve más variaciones del mismo animal y generaliza mejor. (Si, en cambio, tanto train como val fueran bajas, el síntoma sería subajuste y la mejora correcta sería un segundo bloque Conv o más filtros.)

---

## 10 Pregunta 9 — CNN vs MLP vs RNN (mismo problema, mala herramienta)

Tres escenarios. Elige CNN, MLP o RNN/Transformer y justifica en una frase cada uno:

1. Predecir si un alumno aprueba con promedio, faltas y horas de estudio (tabla).
2. Clasificar fotos de semáforo (rojo / amarillo / verde).
3. Autocompletar la siguiente línea de código dado el prefijo escrito.

Si aplastas la foto del semáforo y usas un MLP “grande”, ¿puede aprender? ¿Qué sacrificas respecto a una CNN? (parámetros, datos, invariancia…)

### Respuestas

1.- MLP. Son pocas variables numéricas tabulares sin estructura espacial ni secuencial, así que una red densa pequeña (o incluso un modelo más simple) basta.

2.- CNN. Son imágenes: el color y la forma de la luz se detectan con patrones locales que se repiten en cualquier posición, justo lo que explotan los filtros convolucionales con pesos compartidos.

3.- RNN/Transformer. El código es una secuencia donde lo que sigue depende del contexto previo (orden y dependencias a larga distancia), y estos modelos están diseñados para procesar y generar secuencias.

Sí, puede aprender (es un aproximador universal, y en un caso fácil como distinguir rojo/amarillo/verde incluso puede funcionar razonablemente). Pero se sacrifica:

- Parámetros: cada píxel se conecta a cada neurona, así que el número de pesos se dispara (millones) frente a los pocos cientos o miles de una CNN.
- Datos: con tantos parámetros necesita muchas más imágenes para no sobreajustar.
- Invariancia/equivarianza a la traslación: al aplastar se pierde la estructura 2D; el MLP aprende por separado qué ocurre en cada posición, así que si la luz aparece en otro sitio del encuadre debe reaprenderlo.
- Estructura espacial: no aprovecha que los píxeles vecinos están relacionados, ni la jerarquía de patrones (bordes → formas → objetos).

---

## 11 Pregunta 10 — Experimento mental: mover un gato

Entrenaste una CNN que detecta “hay un gato” en fotos 64×64. En el test, el mismo gato aparece corrido 10 píxeles a la derecha, mismo fondo, misma escala.

### Tareas

1. ¿Por qué una CNN tiene más chance de seguir acertando que un MLP que ve el vector aplastado? (pesos compartidos / detección local)
2. Si en vez de correrlo lo giraras 90° y nunca viste gatos de lado en el train, ¿la CNN “mágica” garantiza acierto? ¿Qué haría falta (augmentation, más datos…)?
3. Escribe una pregunta que tú le harías a un compañero para comprobar si entendió qué son los pesos del kernel (no copies las de arriba).

### Respuestas

1.- En la CNN, el mismo kernel (los mismos pesos) se desliza por toda la imagen, así que un patrón (una oreja, un bigote) produce la misma respuesta sin importar dónde esté; si el gato se corre, el mapa de características se desplaza con él (equivarianza a la traslación), y el pooling aporta algo de invariancia local. En un MLP con el vector aplastado, cada píxel tiene sus propios pesos: el gato corrido activa entradas completamente distintas, para las que la red quizá nunca aprendió nada, y tendría que reaprender el patrón en cada posición.

2.- No hay garantía. La CNN es equivariante a traslaciones, no a rotaciones: los filtros aprendieron orientaciones específicas (orejas arriba, patas abajo), y un gato de lado produce patrones que nunca vio. Haría falta data augmentation con rotaciones, más datos que incluyan gatos en distintas orientaciones, o arquitecturas/técnicas diseñadas para ser equivariantes a rotación.

3.- "Un kernel 3×3 se aplica en cada posición de la imagen: ¿cuántos pesos distintos tiene en total, quién decide sus valores y por qué no cambian de una posición a otra?" (Respuesta esperada: tiene 9 pesos por canal de entrada, más un bias; sus valores los aprende la red por retropropagación al minimizar la pérdida, y son los mismos en todas las posiciones porque se comparten.)

---

## 12 Autoevaluación rápida (opcional)

Marca 1–5 (1 = no entendí, 5 = lo explicaría en pizarrón):

| Tema                               | Nota |
|------------------------------------|------|
| Producto punto del kernel          |      |
| Conteo de parámetros Conv vs Dense |      |
| Efecto de apilar Conv+Pool         |      |
| Softmax + pérdida                  |      |
| Dropout train vs inferencia        |      |

---

## 13 Notas al pie (fórmulas útiles — no sustituyen el razonamiento)

- Salida convolución (un eje), padding valid: floor((N - K)/S) + 1
- Padding same + stride 1: salida ≈ mismo N
- Parámetros Conv2D: Kh · Kw · Cin · Cout + Cout (si hay bias)
- Parámetros Dense: n_in · n_out + n_out (si hay bias)
