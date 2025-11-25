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
	•	Número de transacciones entrantes/salientes en ventanas cortas (24h, 7d).
	•	Monto total recibido en los últimos días (7–30 días).
	•	Antigüedad de la cuenta / del cliente (cuentas muy jóvenes con mucho movimiento son sospechosas).



4. Explica qué es overfitting.
Es cuando el modelo aprende demasiado bien los patrones y el ruido del conjunto de entrenamiento.
Consigue muy buen desempeño en train, pero generaliza mal a datos nuevos (test/producción).



## SECCIÓN 2 – Ejercicio práctico (Python)

A continuación tienes un script listo para usar en Jupyter/Colab. Solo necesitas guardar el CSV (por ejemplo mules_dataset.csv) con el contenido indicado en el enunciado y ejecutar el código.

el código se encuentra en el notebook `technical_test.ipynb`

se requiere de activar el ambiente virtual utilizando el package manager de uv que se puede instalar desde:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```
para una distribución linux/mac. Para más información de comom instalar uv sigan las instrucciones en [este link](https://docs.astral.sh/uv/getting-started/installation/)

para instalar el ambiente virtual y sus dependencias solo debe de ejecutar en una terminal el siguiente comando (uan vez que ya esté instalado `uv`):

```sh
uv sync
```

este comando instalara las dependencias que estén en el `pyproject.toml` y el `uv.lock`


2.4 Métricas obtenidas (con el código anterior)

En el conjunto de test (30% de los datos, random_state=42, estratificado) se obtienen:
	•	Precision: ≈ 0.50
	•	Recall: ≈ 1.00
	•	Accuracy: ≈ 0.67

(Es decir, detecta todos los casos “mule” del test —recall 1.0—, pero con algunos falsos positivos, de ahí la precisión 0.5.)


2.5 Variables más importantes para identificar una cuenta mula

A partir de los coeficientes de la Regresión Logística (en valor absoluto) y del propio sentido del negocio, destacan:
	•	is_young: las cuentas de titulares jóvenes tienen mayor probabilidad de ser clasificadas como mula.
	•	total_received_last_7d: montos muy altos recibidos en pocos días son un fuerte indicador de riesgo.
	•	transactions_last_24h y received_per_txn: muchas transacciones en muy poco tiempo y/o tickets promedio elevados también empujan la probabilidad hacia “mule”.

En resumen, cuentas jóvenes con mucho movimiento reciente y altos montos recibidos son las más sospechosas según el modelo.



## SECCIÓN 3 – Análisis

3.1. Si el modelo genera muchas alertas pero detecta pocas cuentas mula reales, ¿qué ajustarías?
Ajustaría el umbral de decisión (probabilidad mínima para marcar “mule”) haciéndolo más estricto para subir la precisión y bajar falsos positivos.
También revisaría features/modelo y optimizaría respecto a precisión o Fβ (con β<1) en vez de solo accuracy.



3.2. Si tuvieras un dataset con solo 0.2% de cuentas mula, ¿qué técnicas usarías para manejar el desbalance?
	•	Asignar pesos de clase (class_weight) para penalizar más los errores en la clase minoritaria.
	•	Oversampling de la clase minoritaria (p. ej. SMOTE) y/o undersampling de la mayoritaria.
	•	Considerar enfoques de detección de anomalías y usar métricas como ROC-AUC y PR-AUC en lugar de solo accuracy.