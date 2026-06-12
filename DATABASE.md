# Documentación Técnica: Dataset Breast Cancer Wisconsin (Diagnostic)

Este documento presenta la caracterización, auditoría preliminar y decisiones metodológicas aplicadas sobre el conjunto de datos utilizado para el desarrollo del modelo predictivo de clasificación de cáncer de mama.

---

## 1. Origen y Fiabilidad de la Fuente

El conjunto de datos analizado proviene del **UCI Machine Learning Repository** (ID del Dataset: 17), una de las plataformas de alojamiento de datos científicos más prestigiosas y validadas a nivel mundial dentro del ámbito de la inteligencia artificial y la bioinformática. 

### ¿Por qué es una fuente confiable?
* **Validación Científica:** Donado originalmente por los investigadores de la Universidad de Wisconsin (Nick Street, William Wolberg y Olvi Mangasarian), este dataset ha servido como base para cientos de artículos de investigación arbitrados (*peer-reviewed*).
* **Ausencia de Sesgos de Captura:** Los datos se obtuvieron directamente de casos clínicos reales mediante imágenes digitalizadas de muestras tomadas por aspiración con aguja fina (AAF) de masas mamarias, garantizando su autenticidad y utilidad diagnóstica.

---

## 2. Estructura y Dimensiones del Dataset

El dataset original se compone de una matriz de **569 instancias** (filas que representan pacientes) y **32 columnas cuantitativas y cualitativas**.

###  Variable Objetivo (Target)
* **Nombre:** `Diagnosis`
* **Tipo:** Categórica nominal binaria.
* **Clases:**
  * **M** (Maligno): Indica la presencia de tejido canceroso.
  * **B** (Benigno): Indica la presencia de masas no cancerosas.

###  Variables Predictoras (Features)
El dataset extrae **10 características base** de los núcleos celulares presentes en cada imagen digitalizada. Para cada característica, se calculan tres valores estadísticos diferentes (la Media, el Error Estándar y el "Peor" o Máximo valor), dando un total de **30 variables continuas**:

| Característica Base | Descripción Métrica |
| :--- | :--- |
| **Radio (Radius)** | Media de las distancias desde el centro hasta los puntos del contorno. |
| **Textura (Texture)** | Desviación estándar de los valores de la escala de grises. |
| **Perímetro (Perimeter)** | Longitud total del contorno del núcleo celular. |
| **Área (Area)** | Superficie total ocupada por el núcleo celular. |
| **Suavidad (Smoothness)** | Variación local en las longitudes de los radios. |
| **Compacidad (Compactness)** | Calculada matemáticamente como: $(perimetro^2 / area) - 1.0$. |
| **Concavidad (Concavity)** | Gravedad de las porciones cóncavas del contorno. |
| **Puntos Cóncavos (Concave Points)** | Número de porciones cóncavas en el contorno celular. |
| **Simetría (Symmetry)** | Equilibrio geométrico de la forma celular. |
| **Dimensión Fractal (Fractal Dimension)** | Aproximación de la línea fronteriza utilizando la "aproximación de costa". |

---

## 3. Variables Descartadas y Justificación Técnica

Durante la fase de limpieza y preparación en Visual Studio Code, se descartó de forma definitiva la variable **`ID` (Identificador del Paciente)**.

>  **Justificación Académica:** El identificador numérico es un código correlativo de carácter administrativo. Al carecer por completo de relación patológica o biológica con la formación o comportamiento de un tumor, su inclusión en el entrenamiento habría provocado que los modelos de Machine Learning intentaran correlacionar números de cédula/historia clínica con el diagnóstico, induciendo a un error crítico de **sobreajuste (*overfitting*)**.

---

## 4. Auditoría de Valores Atípicos (Outliers)

A través de la inspección de distribución por diagramas de caja (*Boxplots*), se detectó una cantidad significativa de valores extremos (outliers) en las variables de volumen y superficie, tales como `area_worst` y `perimeter_worst`.

### Decisión Metodológica: **No Eliminación**
A diferencia de otros proyectos analíticos donde los outliers se eliminan o imputan, en el diagnóstico oncológico **los valores atípicos se mantuvieron intactos** bajo el siguiente criterio médico-estadístico:
* Las células tumorales malignas sufren mutaciones genéticas que alteran drásticamente su tamaño, provocando que se hinchen o deformen multiplicando su volumen normal.
* Por lo tanto, un valor extremadamente alto en el área o el perímetro no representa un error de medición de la máquina, sino que es la **evidencia biológica más fuerte** de la malignidad. Eliminar estos registros destruiría la capacidad del modelo para detectar los cánceres más agresivos.

---

