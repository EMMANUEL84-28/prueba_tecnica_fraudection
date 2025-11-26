# prueba_tecnica_fraudection
Este repositorio contiene una solución para una prueba técnica orientada a la detección de cuentas mula mediante técnicas básicas de ciencia de datos y Machine Learning.

## SECCIÓN 1 – Conceptos

1. ¿Qué es un dataset desbalanceado y por qué es un problema?
Es un dataset donde una clase (por ejemplo, “mule” = 1) tiene muy pocos ejemplos frente a la otra.
El modelo tiende a “ignorar” la clase minoritaria y métricas como el accuracy se vuelven engañosas.


2. ¿Qué métrica es más importante para detectar fraude: precisión, recall o accuracy? ¿Por qué?
Para fraude suele ser más importante el recall (sensibilidad), porque es muy costoso dejar escapar un caso fraudulento (falso negativo).
El accuracy puede ser alto en datasets muy desbalanceados aunque el modelo casi no detecte fraude.



3. Menciona 3 features útiles para detectar posibles cuentas mula.
	•	Cantidad de transacciones entrantes y salientes en ventanas cortas (24hrs o 7dias).
	•	Monto total recibido en los últimos días (por ejemplo 7–30 días).
	•	Antigüedad de la cuenta del cliente (cuentas muy nuevas con mucho movimiento son sospechosas).


4. Explica qué es overfitting.
Es cuando el modelo aprende demasiado bien los patrones y el ruido del conjunto de entrenamiento.
Consigue muy buen desempeño en train, pero generaliza mal a datos nuevos como los del test o produccion.



## SECCIÓN 2 – Ejercicio práctico (Python)

Se utilizó el notebook technical_test.ipynb para ejecutar el análisis. Los datos se cargaron desde un CSV (mules_dataset.csv). Para asegurar que las librerías necesarias estuvieran disponibles, se creó un ambiente virtual y se instalaron las dependencias indicadas en el proyecto.
```

2.4 Métricas del modelo en el conjunto de test:

En el conjunto de test (30% de los datos, random_state=42, estratificado) se obtienen:
	•	Recall (sensibilidad): 1.0 → El modelo detecta todos los casos de “mula”, lo cual es importante para no dejar fraude sin identificar.
	•	Precisión: ≈0.5 → Algunos casos que no son “mula” fueron clasificados como sospechosos (falsos positivos).
	•	Accuracy: ≈0.67 → Indica el desempeño general del modelo, aunque en este tipo de datasets desbalanceados el recall es más relevante.

(El modelo prioriza no perder ningún caso de fraude, aunque eso implique marcar algunos casos normales como sospechosos. Esto es coherente con el objetivo de detección de cuentas “mula”.)


2.5 Variables más importantes para identificar una cuenta mula

A partir de los coeficientes de la Regresión Logística (en valor absoluto) y del propio sentido del negocio, destacan:
	•	Edad de la cuenta / titular: Las cuentas nuevas o de titulares jóvenes tienden a ser más sospechosas.
	•	Montos recibidos recientemente: Cantidades altas recibidas en pocos días aumentan el riesgo de ser “mula”.
	•	Actividad reciente: Muchas transacciones en poco tiempo o tickets promedio altos también son señales de alerta..

En resumen, según los datos y el comportamiento de las cuentas, las más sospechosas son aquellas que son jóvenes, tienen mucho movimiento reciente y reciben montos altos.



## SECCIÓN 3 – Análisis

3.1. Si el modelo genera muchas alertas pero detecta pocas cuentas mula reales, ¿qué ajustarías?
Ajustaría el umbral de decisión para que solo las cuentas con alta probabilidad sean marcadas como “mula”.
También revisaría las variables usadas para reducir falsos positivos y hacer las alertas más confiables.




3.2. Si tuvieras un dataset con solo 0.2% de cuentas mula, ¿qué técnicas usarías para manejar el desbalance?
	•	Dar mayor peso a la clase minoritaria para que el modelo preste más atención a los errores en las cuentas “mula”.
	•	Aumentar los ejemplos de la clase minoritaria o reducir los de la clase mayoritaria para que el modelo vea más balance entre cuentas normales y “mula”.
	•	Considerar enfoques de detección de anomalías y usar métricas como ROC-AUC y PR-AUC en lugar de solo accuracy.
