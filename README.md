
# Aprendizaje Automático para la Detección Temprana de Cáncer de Mama
## Proyecto de Clasificación empleando el Dataset Breast Cancer Wisconsin (Diagnostic)

Este repositorio contiene el pipeline completo de ciencia de datos y aprendizaje automático desarrollado localmente en Visual Studio Code para la identificación predictiva de tumores mamarios (Benignos y Malignos). El flujo abarca desde la adquisición automatizada de datos hasta el diagnóstico predictivo y la evaluación multimétrica del rendimiento de los clasificadores.

---

## 1. Introducción y Contexto Histórico

El cáncer de mama constituye uno de los desafíos de salud pública más apremiantes de la historia contemporánea. Históricamente, se ha posicionado como el tipo de carcinoma más diagnosticado a nivel mundial y la principal causa de mortalidad por cáncer en la población femenina. Tradicionalmente, la detección dependía de evaluaciones clínicas manuales y análisis histopatológicos subjetivos, lo que solía derivar en diagnósticos tardíos o en la variabilidad interobservador entre patólogos.

La importancia de este proyecto radica en la intersección de la oncología médica y el análisis computacional. Un diagnóstico temprano altera drásticamente el pronóstico clínico: la tasa de supervivencia a 5 años supera el 90% si la neoplasia se detecta en etapas localizadas, mientras que desciende críticamente si se identifica en fases metastásicas. Al desarrollar este sistema automatizado, se provee una herramienta de soporte analítico de alta precisión capaz de mitigar el error humano, optimizar los tiempos de respuesta hospitalaria y objetivizar la separación diagnóstica entre masas benignas y malignas utilizando técnicas reproducibles de Machine Learning.

---

## 2. Análisis Exploratorio de Datos (EDA) y Auditoría Inicial

Antes de realizar cualquier transformación matemática, se llevó a cabo una auditoría exhaustiva de la estructura de la base de datos para comprender la naturaleza íntima del fenómeno:

* **Inspección de Tipos y Consistencia:** Se verificó que las 30 variables predictoras correspondieran a variables continuas cuantitativas de escala flotante (`float64`), mientras que la variable objetivo (`diagnosis`) se identificó como categórica nominal binaria. Se auditó la matriz confirmando la ausencia absoluta de valores nulos o registros duplicados, garantizando la integridad metodológica inicial.
* **Estudio de Correlación Lineal:** Se estructuraron matrices de correlación de Pearson entre parejas de variables. Este paso evidenció una fuerte colinealidad intrínseca (redundancia lineal) entre las métricas geométricas básicas; por ejemplo, el radio, el perímetro y el área celular mostraron coeficientes de correlación cercanos a $r = 1.0$.
* **Auditoría de Valores Atípicos (Outliers):** A través de diagramas de caja (*Boxplots*), se identificó una cantidad importante de valores extremos en variables de volumen como `area_worst`. De acuerdo con el rigor académico biomédico, se determinó no eliminar estos outliers. En oncología, las células malignas se caracterizan biológicamente por su crecimiento desproporcionado y deformidad; por lo tanto, estos extremos numéricos son firmas patológicas reales esenciales para predecir la malignidad.

---

## 3. Reducción de Dimensionalidad No Supervisada (PCA)

Dada la alta dimensionalidad del espacio original (30 variables predictoras), la visualización directa del espacio de características resulta inviable para el entendimiento humano. 

Se aplicó el método de **Análisis de Componentes Principales (PCA)** parametrizado estrictamente a 2 dimensiones (2D) con un fin meramente exploratorio y de control analítico. PCA proyectó la varianza ortogonal de las 30 dimensiones sobre dos ejes artificiales incorrelacionados (Componente Principal 1 y Componente Principal 2). Al mapear el resultado en un Scatter Plot interactivo, se demostró empíricamente la presencia de un alto grado de separabilidad lineal entre los cúmulos (*clusters*) de pacientes benignos y malignos, justificando sólidamente el uso posterior de algoritmos de clasificación supervisados.

---

## 4. Partición de Datos, Escalamiento y Benchmarking de Modelos

Para asegurar una evaluación honesta, libre de sesgos y mitigar el sobreajuste (*overfitting*), se ejecutó un protocolo riguroso de preparación de datos:

1. **Partición Matricial Rígida (70% / 15% / 15%):** Los datos se dividieron en tres subconjuntos independientes:
   * **Entrenamiento (70%):** Destinado al ajuste de pesos y parámetros de los modelos.
   * **Validación (15%):** Utilizado para el ajuste fino de hiperparámetros y la prevención de sobreajuste.
   * **Testeo (15%):** Reservado exclusivamente como un conjunto ciego final para auditar la capacidad de generalización real de los algoritmos.
2. **Escalamiento de Características:** Se aplicó una estandarización basada en Z-score mediante **`StandardScaler`**. Este paso reajustó las medias a 0 y las desviaciones estándar a 1. Esto evitó que variables con magnitudes colosales (como el área, $>2500\text{ mm}^2$) absorbieran artificialmente el peso computacional de variables con escalas decimales críticas (como la suavidad, $<0.15$), un paso mandatorio para el correcto funcionamiento de clasificadores geométricos o lineales.
3. **Benchmarking de Modelos:** Se entrenaron y evaluaron 4 arquitecturas algorítmicas de distinta naturaleza matemática:
   * **Regresión Logística (Logistic Regression):** Como línea base de clasificación lineal.
   * **Bosques Aleatorios (Random Forest Classifier):** Como ensamble robusto basado en árboles de decisión no lineales.
   * **Máquinas de Soporte Vectorial (SVC):** Para la búsqueda de hiperplanos óptimos de separación en espacios de alta dimensión.
   * **K-Vecinos Más Cercanos (KNN):** Como clasificador basado en distancias geométricas locales.

---

##  5. Selección Óptima, Curvas de Aprendizaje y Matriz de Confusión

Tras someter a los 4 candidatos al banco de pruebas con el conjunto de validación, se seleccionó el **modelo más óptimo** balanceando precisión general, estabilidad y, sobre todo, el costo de los errores médicos. En este dominio, los Falsos Negativos (clasificar a una paciente enferma como sana) representan el escenario más crítico.

Para el modelo seleccionado, se generaron las siguientes herramientas avanzadas de diagnóstico de rendimiento:

* **Métricas de Rendimiento Clásicas:** Se calcularon de forma estricta las métricas de precisión, sensibilidad (*Recall*), especificidad, puntuación F1 (*F1-Score*) y el área bajo la curva ROC (*AUC-ROC*), garantizando un diagnóstico holístico de su comportamiento.
* **Curvas de Entrenamiento y Aprendizaje (Learning Curves):** Se graficó la evolución del rendimiento del modelo contrastando el conjunto de entrenamiento frente al de validación a medida que aumentaba el volumen de muestras. Esto permitió validar visualmente la convergencia del modelo, demostrando matemáticamente la ausencia de subajuste (*underfitting*) o sobreajuste (*overfitting*).
* **Matriz de Confusión Final:** Se computó la matriz de confusión sobre el conjunto de testeo independiente para cuantificar de manera exacta la distribución de Verdaderos Positivos, Verdaderos Negativos, Falsos Positivos y Falsos Negativos. El modelo demostró una alta sensibilidad, minimizando casi a cero los falsos negativos, consolidándose como una herramienta confiable de soporte clínico.

---

##  Requisitos para el Entorno Local (VS Code)

Para ejecutar el código analítico desarrollado en este proyecto, asegúrese de tener configurado el archivo `requirements.txt` con las versiones estables utilizadas:

```text
ucimlrepo==0.0.7
numpy==1.26.4
pandas==2.2.2
matplotlib==3.8.4
seaborn==0.13.2
plotly==5.22.0
kaleido==0.1.0.post1
scikit-learn==1.4.2
nbformat>=4.2.0
ipython