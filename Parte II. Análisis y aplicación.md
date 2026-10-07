\# Parte II. Análisis y aplicación



\## Pregunta 6

\*\*Una institución bancaria desea desarrollar un sistema que detecte posibles compras fraudulentas. (Variables: Monto, Hora, Ciudad, Tipo de establecimiento, Número de compras, Historial). Analice las ventajas y desventajas de utilizar un árbol de decisión y una red neuronal multicapa. ¿Cuál utilizaría y por qué?\*\*



\*   \*\*Árbol de Decisión:\*\*

&#x20;   \*   \*Ventajas:\* Alta interpretabilidad (si un cliente reclama, el banco puede decirle exactamente qué regla disparó la alerta, ej. "Monto > $5000 a las 3 AM en otra ciudad"). Es rápido de entrenar y no requiere escalar los datos.

&#x20;   \*   \*Desventajas:\* Tiende a sobreajustarse (overfitting) y le cuesta detectar patrones no lineales muy complejos que los defraudadores cambian constantemente.

\*   \*\*Red Neuronal Multicapa (MLP):\*\*

&#x20;   \*   \*Ventajas:\* Excelente para encontrar patrones sutiles y ocultos entre múltiples variables (ej. la combinación exacta de historial y frecuencia de compra). Generalmente ofrece mayor precisión en detección de anomalías complejas.

&#x20;   \*   \*Desventajas:\* Es una "caja negra" (difícil explicar por qué bloqueó la tarjeta). Requiere muchos más datos de entrenamiento y mayor poder computacional.



\*\*¿Cuál utilizaría y por qué?\*\*

En un entorno puramente orientado a \*detectar\* la mayor cantidad de fraudes (donde los patrones evolucionan rápido), utilizaría la \*\*Red Neuronal Multicapa\*\*. El fraude financiero es altamente complejo y no lineal. Sin embargo, en la práctica bancaria estricta, la interpretabilidad es vital por cuestiones regulatorias. Si me limitan a usar un solo modelo puro y la regulación exige explicaciones, optaría por el árbol (o un ensamble basado en árboles). Pero si el objetivo principal es la reducción de pérdidas económicas por detección precisa, la red neuronal es superior.



\---



\## Pregunta 7

\*\*Una escuela quiere detectar estudiantes que presentan riesgo de reprobar. (Variables: Asistencia, Calificaciones, Tareas, Participación, Reprobadas anteriores). Suponga que ambos modelos obtienen prácticamente la misma precisión. ¿Qué otros factores tomaría en cuenta para elegir uno? Justifique su respuesta.\*\*



Si la precisión es la misma, elegiría definitivamente el \*\*Árbol de Decisión\*\*. Los factores determinantes para esta elección son:

1\.  \*\*Interpretabilidad y Accionabilidad:\*\* El propósito de predecir el riesgo no es solo "saberlo", sino \*intervenir\*. Un árbol de decisión genera reglas claras. Un profesor puede ver que un alumno está en riesgo porque "Asistencia < 80% y Tareas entregadas < 5". Esto permite crear un plan de acción específico para ayudar al estudiante. Una red neuronal no diría qué área específica debe mejorar el alumno.

2\.  \*\*Costo Computacional y Mantenimiento:\*\* Los árboles de decisión son modelos más ligeros, rápidos y fáciles de implementar o actualizar en los sistemas escolares, que generalmente no cuentan con gran infraestructura tecnológica.



\---



\## Pregunta 8

\*\*Un hospital desarrolla un sistema para determinar prioridad de pacientes (Triage). Una red neuronal obtiene mejores resultados, pero es más difícil explicar su respuesta. ¿Considera que la mayor precisión es suficiente para elegir la red neuronal? Analice las consecuencias.\*\*



\*\*No, la mayor precisión estadística rara vez es suficiente por sí sola en el ámbito de la salud.\*\* En medicina clínica, el contexto es de "alto riesgo" (high stakes).



\*\*Consecuencias de la decisión:\*\*

Si se elige la red neuronal (caja negra), los médicos no podrán auditar el razonamiento detrás de la asignación de prioridad. Si el sistema comete un error grave (ej. manda a la sala de espera a alguien a punto de sufrir un infarto), será imposible justificar clínicamente o legalmente por qué ocurrió. Esto introduce problemas de \*responsabilidad médica\* y \*sesgo algorítmico\* invisible. Los médicos necesitan herramientas de apoyo a la decisión, no oráculos inescrutables; requieren entender el porqué para poder confiar en el modelo o anularlo basándose en su propia experiencia clínica (interpretabilidad por encima de la precisión marginal).



\---



\## Pregunta 9

\*\*Empresa de reparto quiere predecir retrasos. Para un pedido, el Árbol indica "A tiempo" y la Red Neuronal "Tarde". ¿Cómo determinaría cuál hace una mejor predicción? Explique qué información adicional debería analizar.\*\*



No se puede determinar cuál es mejor evaluando un solo caso aislado. Para saber cuál modelo es superior, se debe realizar una evaluación rigurosa usando un conjunto de datos de validación/prueba que los modelos no hayan visto antes. 

Debería analizar la siguiente información adicional:

\*   \*\*Métricas de desempeño global:\*\* Comparar la Exactitud (Accuracy), Precisión, Sensibilidad (Recall) y el F1-Score de ambos modelos sobre miles de viajes históricos.

\*   \*\*Matriz de confusión:\*\* Observar qué modelo tiene más "falsos positivos" (decir que llega tarde y llegó a tiempo) y "falsos negativos" (decir que llega a tiempo y llegó tarde). ¿Qué error le cuesta más a la empresa de reparto?

\*   \*\*Probabilidades de clase (Confianza):\*\* Las redes neuronales no solo arrojan una clase, arrojan una probabilidad (ej. 51% de llegar tarde vs 99%). Revisar la certeza de ambos modelos para ese pedido en particular.



\---



\## Pregunta 10

\*\*Sistema de créditos. Árbol: Explicable. Red Neuronal: Más precisa pero

