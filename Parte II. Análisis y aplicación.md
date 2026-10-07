# Parte II. Análisis y aplicación

## Pregunta 6
**Una institución bancaria desea desarrollar un sistema que detecte posibles compras fraudulentas.**
**El sistema dispone de información como:**
* **Monto de la compra.**
* **Hora de la operación.**
* **Ciudad donde se realizó.**
* **Tipo de establecimiento.**
* **Número de compras realizadas durante el día.**
* **Historial de compras del cliente.**
**Analice las ventajas y desventajas de utilizar un árbol de decisión y una red neuronal multicapa.**
**¿Cuál utilizaría y por qué?**

**Árbol de Decisión:** Tiene la ventaja de una alta interpretabilidad, es rápido de entrenar y no requiere escalar los datos. Su desventaja es que tiende a sobreajustarse y le cuesta detectar patrones no lineales muy complejos que los defraudadores cambian constantemente.
**Red Neuronal Multicapa:** Tiene la ventaja de ser excelente para encontrar patrones sutiles y ocultos entre múltiples variables. Generalmente ofrece mayor precisión en detección de anomalías complejas. Su desventaja es que es una "caja negra". Requiere muchos más datos de entrenamiento y mayor poder computacional.

Utilizaría la red neuronal multicapa. El fraude financiero es altamente complejo y no lineal. Sin embargo, en la práctica bancaria estricta, la interpretabilidad es vital por cuestiones regulatorias. Si me limitan a usar un solo modelo puro y la regulación exige explicaciones, optaría por el árbol. Pero si el objetivo principal es la reducción de pérdidas económicas por detección precisa, la red neuronal es superior.

---

## Pregunta 7
**Una escuela quiere detectar estudiantes que presentan riesgo de reprobar una materia.**
**Se conocen variables como:**
* **Asistencia.**
* **Calificaciones.**
* **Tareas entregadas.**
* **Participación.**
* **Número de materias reprobadas anteriormente.**
**Suponga que un árbol de decisión y una red neuronal obtienen prácticamente la misma precisión.**
**¿Qué otros factores tomaría en cuenta para elegir uno de los dos modelos?**
**Justifique su respuesta.**

Elegiría el árbol de decisión. Tomaría en cuenta principalmente la interpretabilidad y accionabilidad. El propósito de predecir el riesgo no es solo saberlo, sino intervenir. Un árbol de decisión genera reglas claras; un profesor puede ver exactamente por qué un alumno está en riesgo y esto permite crear un plan de acción específico para ayudar al estudiante. Una red neuronal no diría qué área específica debe mejorar el alumno. También tomaría en cuenta el costo computacional, ya que los árboles son modelos más ligeros y fáciles de implementar en los sistemas escolares.

---

## Pregunta 8
**Un hospital desarrolla un sistema para determinar qué pacientes necesitan atención prioritaria utilizando:**
* **Edad.**
* **Temperatura.**
* **Presión arterial.**
* **Frecuencia cardiaca.**
* **Síntomas.**
* **Antecedentes médicos.**
**Una red neuronal obtiene mejores resultados que un árbol de decisión, pero resulta más difícil explicar cómo obtuvo su respuesta.**
**¿Considera que la mayor precisión es suficiente para elegir la red neuronal?**
**Analice las consecuencias que podría tener esta decisión.**

No, la mayor precisión estadística rara vez es suficiente por sí sola en el ámbito de la salud. En medicina clínica, el contexto es de alto riesgo.

Si se elige la red neuronal, los médicos no podrán auditar el razonamiento detrás de la asignación de prioridad. Si el sistema comete un error grave, será imposible justificar clínicamente o legalmente por qué ocurrió. Esto introduce problemas de responsabilidad médica y sesgo algorítmico invisible. Los médicos necesitan herramientas de apoyo a la decisión, no oráculos inescrutables; requieren entender el porqué para poder confiar en el modelo o anularlo basándose en su propia experiencia clínica.

---

## Pregunta 9
**Una empresa de reparto quiere predecir si un pedido llegará tarde considerando:**
* **Distancia.**
* **Tráfico.**
* **Clima.**
* **Hora del día.**
* **Cantidad de pedidos.**
* **Experiencia del repartidor.**
**Para determinado pedido, el árbol de decisión indica:**
**Llegará a tiempo**
**mientras que la red neuronal indica:**
**Probablemente llegará tarde**
**¿Cómo determinaría cuál de los dos modelos está realizando una mejor predicción?**
**Explique qué información adicional debería analizar.**

Siento que no se puede saber cuál modelo es mejor basándonos en un solo pedido aislado. En un proyecto real, lo que haría sería evaluar ambos algoritmos usando un conjunto de datos de prueba que ninguno de los modelos haya visto antes, tal como hacemos cuando analizamos datasets en pandas durante las prácticas. Para tomar una decisión bien fundamentada, calcularía las métricas generales de desempeño sobre miles de viajes históricos, revisando la exactitud, la precisión y el F1-Score. Además, generaría una matriz de confusión para analizar los falsos positivos y falsos negativos, porque ahí es donde te das cuenta de qué error le afecta más a la empresa, por ejemplo, si es peor prometer que el paquete llega a tiempo y fallar, o decir que llegará tarde y sorprender al cliente. Finalmente, también me fijaría en las probabilidades. Como la red neuronal te da un porcentaje de confianza y no solo el resultado final, analizaría qué tan seguros estaban los dos modelos al hacer su predicción para ese viaje en específico antes de decidir en cuál confiar más.

---

## Pregunta 10
**Una empresa desarrolla dos sistemas para decidir si una persona puede recibir un crédito.**
**El primer sistema utiliza un árbol de decisión y permite explicar claramente por qué una solicitud fue rechazada.**
**El segundo utiliza una red neuronal multicapa y obtiene mejores resultados de predicción, pero es más difícil explicar sus decisiones.**
**Si usted fuera responsable del proyecto:**
**¿Cuál de los dos modelos utilizaría?**
**¿Qué ventajas tendría su elección?**
**¿Qué riesgos tendría?**
**¿Consideraría posible utilizar ambos modelos dentro del mismo sistema?**
**Justifique ampliamente su respuesta.**

Si yo fuera el responsable del proyecto, creo que intentaría implementar un sistema híbrido que aproveche ambos modelos. Dejaría la red neuronal multicapa como el motor principal para evaluar los créditos, ya que nos va a dar mejores predicciones y ayudará al banco a perder menos dinero al evitar fraudes o impagos. Sin embargo, como en los bancos existen regulaciones estrictas y los clientes necesitan saber por qué se les rechazó el crédito, usaría el árbol de decisión en paralelo como una herramienta de apoyo para poder explicar de forma clara qué variables provocaron el rechazo. La principal ventaja de esta elección es que logramos un equilibrio entre obtener la máxima precisión posible y mantener la transparencia que exige el negocio. El riesgo que le veo a esta solución es que la arquitectura de software del proyecto se vuelve bastante más pesada y compleja de mantener, además de que requeriría más procesamiento en los servidores.

---

**A partir de los ejercicios anteriores, explique brevemente la siguiente afirmación:**
**No existe un algoritmo de Inteligencia Artificial que sea el mejor para todos los problemas.**
**Relacione su respuesta con los conceptos de:**
* **Precisión.**
* **Interpretabilidad.**
* **Cantidad de datos.**
* **Complejidad del problema.**
* **Consecuencias de una decisión incorrecta.**

Sobre la afirmación de que no existe un algoritmo de inteligencia artificial que sea el mejor para todo, los casos anteriores demuestran que siempre hay que hacer sacrificios dependiendo del proyecto. Cuando tenemos problemas de alta complejidad y una gran cantidad de datos, las redes neuronales son excelentes para alcanzar una precisión altísima. Pero a veces la interpretabilidad del modelo es mucho más valiosa. Si las consecuencias de una decisión incorrecta son muy graves, como en el caso del hospital o en la negación de servicios financieros, no nos sirve de nada tener un modelo súper preciso si no podemos auditar o explicar cómo llegó a esa conclusión. Por eso, creo que como desarrolladores siempre debemos analizar el contexto antes de elegir qué modelo entrenar, buscando el punto intermedio ideal que resuelva el problema real de forma segura y eficiente.